---
title: "Datastructures: Merkle tree"
date: 2022-07-12T13:33:37Z
---

Summary
-------

Merkle trees (aka hash trees) are tree-like datastructures, that separate data
validation from the the data itself. These trees consist of the following items:

  - Leaves (aka outer nodes aka leaf node)
    - Data objects, that are labelled with: hash(associated_data).
  - Branches (aka inner nodes aka inodes aka non-leaf node)
    - Data objects, that are labelled with: hash(concatenate(child_node_hashes)).



```
                      ┌──────────┐
                      │    H7    │                              Merkle root
                      │ H(H5|H6) │
             ┌────────┴──────────┴──────────┐
             │                              │
             │                              │
        ┌────┴─────┐                  ┌─────┴────┐
        │    H5    │                  │    H6    │              Branches
        │ H(H1|H2) │                  │ H(H3|H4) │
        └─┬─────┬──┘                  └─┬──────┬─┘
          │     │                       │      │
┌─────────┴┐   ┌┴─────────┐    ┌────────┴─┐  ┌─┴────────┐
│   H1     │   │    H2    │    │    H3    │  │    H4    │       Leaves
│  H(A1)   │   │   H(A2)  │    │   H(A3)  │  │   H(A4)  │
└───┬──────┘   └────┬─────┘    └────┬─────┘  └────┬─────┘
    │               │               │             │
    A1              A2              A3            A4            Data

```

Benefits
---------

  -  They provide an efficient way of proving the integrity and validity of data. Even
  a small tamper of data will result in a completely different hash.
  -  Their proofs only require tiny amounts of information to be transmitted across
  networks. They separate the validation of data from the data itself.


Implementation
--------------

```rust
use sha2::Digest;

pub type Data = Vec<u8>;
pub type Hash = Vec<u8>;

fn hash_data(data: &Data) -> Hash {
    sha2::Sha256::digest(data).to_vec()
}

fn hash_concat(h1: &Hash, h2: &Hash) -> Hash {
    let h3 = h1.iter().chain(h2).copied().collect();
    hash_data(&h3)
}

#[derive(Clone)]
pub struct TreeNode {
    left: Option<Box<TreeNode>>,
    right: Option<Box<TreeNode>>,
    value: Data,
    digest: Hash,
}

impl TreeNode {
    fn new(value: &Data) -> Self {
        Self {
            left: None,
            right: None,
            value: value.to_owned(),
            digest: hash_data(value),
        }
    }
}

pub struct MerkleTree {
    root: Option<TreeNode>,
}

impl MerkleTree {
    /// Constructs a Merkle tree from given input data
    pub fn construct(input: &[Data]) -> MerkleTree {
        let mut nodes = vec![];
        for leaf in input.iter() {
            nodes.push(TreeNode::new(leaf))
        }

        while nodes.len() > 1 {
            let mut buf = vec![];
            for i in (0..nodes.len()).step_by(2) {
                let left_node = &nodes[i];
                let right_node;

                if i + 1 < nodes.len() {
                    right_node = nodes[i + 1].clone();
                } else {
                    buf.push(nodes[i].clone());
                    break;
                }

                let digest = hash_concat(&left_node.value, &right_node.value);
                let mut parent = TreeNode::new(&digest);
                parent.left = Some(Box::new(left_node.clone()));
                parent.right = Some(Box::new(right_node));
                buf.push(parent);
            }
            nodes = buf;
        }

        let mut root = None;
        if !nodes.is_empty() {
            root = Some(nodes.remove(0))
        }

        MerkleTree { root: root }
    }

    /// Verifies that the given input data produces the given root hash
    pub fn verify(input: &[Data], root_hash: &Hash) -> bool {
        let tree = Self::construct(input);
        match tree.root {
            Some(root_node) => &root_node.digest == root_hash,
            None => false,
        }
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_construct_empty_tree() {
        let input = [];
        let tree = MerkleTree::construct(&input[..]);
        assert_eq!(tree.root.is_some(), false)
    }

    #[test]
    fn test_construct_populated_tree() {
        let input = [vec![0x41u8], vec![0x42u8], vec![0x43u8], vec![0x44u8]];
        let tree = MerkleTree::construct(&input[..]);
        assert_eq!(tree.root.is_some(), true)
    }

    #[test]
    fn test_verify_root_hash() {
        let input = [vec![0x41u8], vec![0x42u8], vec![0x43u8], vec![0x44u8]];
        let expected_root_hash = vec![
            223u8, 64u8, 230u8, 210u8, 176u8, 51u8, 54u8, 162u8, 150u8, 30u8, 243u8, 139u8, 43u8,
            100u8, 76u8, 36u8, 99u8, 18u8, 6u8, 191u8, 23u8, 251u8, 152u8, 245u8, 123u8, 192u8,
            125u8, 123u8, 87u8, 54u8, 209u8, 84u8,
        ];
        assert_eq!(MerkleTree::verify(&input[..], &expected_root_hash), true)
    }
}
```
