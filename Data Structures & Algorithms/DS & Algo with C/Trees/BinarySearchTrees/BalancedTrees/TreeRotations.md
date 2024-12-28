### Tree rotations

- The problem is that as we insert new nodes, some paths through the tree may become very long.
- So we need to be able to shrink the long paths by moving nodes elsewhere in the tree.
- The idea is to notice that there may be many binary search trees that contain the same data and that we can transform one into another by a local modification called a **rotation**.

There are two primary types:

1. **Left Rotation (LL Rotation)**: Rotates the tree to the left when the right subtree is taller.
2. **Right Rotation (RR Rotation)**: Rotates the tree to the right when the left subtree is taller.

**Types of Rotations**

1. **Left-Left (LL) Rotation**: When a node is inserted in the left subtree of the left child.
2. **Right-Right (RR) Rotation**: When a node is inserted in the right subtree of the right child.
3. **Left-Right (LR) Rotation**: When a node is inserted in the right subtree of the left child.
4. **Right-Left (RL) Rotation**: When a node is inserted in the left subtree of the right child.

![Screenshot from 2024-12-17 20-36-35](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-17 20-36-35.png)

- If A < x < B < y < C, then both versions of this tree have the binary search tree property. By doing the rotation in one direction, we move A up and C down; in the other direction, we move A down and C up.
- So rotations can be used to transfer depth from the leftmost grandchild of a node to the right most and vice versa.
- But what if it's the middle grandchild B that's the problem?
- A single rotation as above doesn't move B up or down.
- To move B, we have to reposition it so that it's on the end of something.
- We do this by splitting B into two subtrees B1 and B2 and doing two rotations that split the two subtrees while moving both up.

![Screenshot from 2024-12-17 22-00-43](/home/chan/Pictures/Screenshots/Screenshot from 2024-12-17 22-00-43.png)

#### Left Rotation

```css
      X
       \
        Y
       / \
     T2  T3
```

- **Identify the Imbalanced Node (`X`)**:
  - The node `X` has a right child `Y` that causes the imbalance.
- **Perform the Rotation**:
  - **Step 1**: `Y` becomes the new root of the subtree.
  - **Step 2**: `X` becomes the left child of `Y`.
  - **Step 3**: The left child of `Y` (`T2`) becomes the right child of `X`.
- **Update Pointers**:
  - Ensure that the parent of `X` now points to `Y` instead of `X`.
  - Update the subtree pointers accordingly.

**Before and After**

**Before Left Rotation:**

```css
      X
       \
        Y
       / \
     T2  T3
```

After Left Rotation:

```css
        Y
       / \
      X   T3
       \
        T2
```



#### Right Rotation

```css
	A
   / \
   B  C	
  / \
 D   E
```

We start with the pointer reference to Node A. Then we'll want a pointer to node B. After that, set A's left pointer to point to B's right child. 

```css
   A
  /
 E
```

Then change B's right pointer to Node A and we have successfully done a right rotation.

```css
B
 \
  A
```

If we rearrange the nodes ,this is what we would end up with

```css
          B
		 / \
		D   A
		   / \
		  E   C
```

#### LL Rotation

**LL Rotation** is a specific case of **Right Rotation** used in **AVL Trees** when a node becomes unbalanced due to the insertion of a node in the **left subtree of its left child**.

**Step-by-Step Process**

```css
        Z
       /
      Y
     /
    X
```

1. **Identify the Imbalance**:
   - The imbalance occurs at node `Z` because a node was inserted into the left subtree of `Z`'s left child `Y`.
2. **Perform Right Rotation on `Z`**:
   - This single right rotation restores balance.
3. **Resulting Structure**:
   - The left child `Y` becomes the new root of the subtree.
   - The original root `Z` becomes the right child of `Y`.
   - The subtree `T2` (originally `Y`'s right child) becomes `Z`'s left child.

**After LL Rotation**:

```css
      Y
     / \
    X   Z
```

#### RR Rotation

**RR Rotation** is a specific case of **Left Rotation** used in **AVL Trees** when a node becomes unbalanced due to the insertion of a node in the **right subtree of its right child**.

**Step-by-Step Process**

```css
    X
     \
      Y
       \
        Z
```

1. **Identify the Imbalance**:
   - The imbalance occurs at node `X` because a node was inserted into the right subtree of `X`'s right child `Y`.
2. **Perform Left Rotation on `X`**:
   - This single left rotation restores balance.
3. **Resulting Structure**:
   - The right child `Y` becomes the new root of the subtree.
   - The original root `X` becomes the left child of `Y`.
   - The subtree `T2` (originally `Y`'s left child) becomes `X`'s right child.

**After RR rotation**:

```css
      Y
     / \
    X   Z
```

#### When to Use Each Rotation

Understanding **when** to apply each rotation is as important as knowing **how** to perform them. Here's when each rotation is typically used:

1. **Left Rotation**:
   - Applied when a node's right subtree is heavier.
   - Commonly used in AVL Trees and Red-Black Trees to rebalance the tree after insertions or deletions that skew the tree to the right.
2. **Right Rotation**:
   - Applied when a node's left subtree is heavier.
   - Used in AVL Trees and Red-Black Trees to rebalance the tree after operations that skew the tree to the left.
3. **LL Rotation**:
   - A specific case of right rotation used in AVL Trees when the imbalance is caused by the left subtree of the left child.
   - Simple single right rotation suffices to rebalance.
4. **RR Rotation**:
   - A specific case of left rotation used in AVL Trees when the imbalance is caused by the right subtree of the right child.
   - Simple single left rotation suffices to rebalance.

#### Double Rotations

Double rotations come into play when a single rotation isn't sufficient to rebalance the tree. Specifically, they address **zig-zag** imbalance patterns where a node's child is heavy on the opposite side of its own imbalance.

**Scenarios Requiring Double Rotations:**

1. **Left-Right (LR) Rotation**: Occurs when a node has a left child that is right-heavy.
2. **Right-Left (RL) Rotation**: Occurs when a node has a right child that is left-heavy.

##### Left-Right (LR) Rotation

An **Left-Right (LR) Rotation** is a combination of a **Left Rotation** followed by a **Right Rotation**. It is used to fix an imbalance where a node's left child is right-heavy.

**Step-by-Step Process:**

**Before LR Rotation:**

```css
        Z
       /
      Y
       \
        X
```

1. **Identify the Imbalanced Node (`Z`)**:

   - Node `Z` has a left child `Y`.
   - Node `Y` has a right child `X`.
   - This creates a zig-zag pattern: `Z` → `Y` (left) → `X` (right).

2. **First Rotation - Left Rotation on `Y`**:

   - Perform a **Left Rotation** on `Y` to transform the subtree into a straight line.
   - After this rotation, `X` becomes the left child of `Z`, and `Y` becomes the left child of `X`.

   ```css
           Z
          /
         X
        /
       Y
   ```

   

3. **Second Rotation - Right Rotation on `Z`**:

   - Perform a **Right Rotation** on `Z` to elevate `X` as the new root of the subtree.
   - `Z` becomes the right child of `X`, and `Y` remains as the left child of `X`.

   ```css
         X
        / \
       Y   Z
   ```

   

##### Right-Left (RL) Rotation:

A **Right-Left (RL) Rotation** is a combination of a **Right Rotation** followed by a **Left Rotation**. It is used to fix an imbalance where a node's right child is left-heavy.

**Step-by-Step Process:**

**Before RL Rotation:**

```css
    Z
     \
      Y
     /
    X
```

1. **Identify the Imbalanced Node (`Z`)**:

   - Node `Z` has a right child `Y`.
   - Node `Y` has a left child `X`.
   - This creates a zig-zag pattern: `Z` → `Y` (right) → `X` (left).

2. **First Rotation - Right Rotation on `Y`**:

   - Perform a **Right Rotation** on `Y` to transform the subtree into a straight line.
   - After this rotation, `X` becomes the right child of `Z`, and `Y` becomes the right child of `X`.

   ```css
       Z
        \
         X
          \
           Y
   ```

   

3. **Second Rotation - Left Rotation on `Z`**:

   - Perform a **Left Rotation** on `Z` to elevate `X` as the new root of the subtree.
   - `Z` becomes the left child of `X`, and `Y` remains as the right child of `X`.

   ```css
         X
        / \
       Z   Y
   ```

##### When to Use Double Rotations

Understanding **when** to apply each double rotation is crucial for maintaining tree balance. Here's when to use each:

1. **Left-Right (LR) Rotation**:
   - **Condition**: A node `Z` has a left child `Y`, and `Y` has a right child `X`.
   - **Imbalance**: Insertion in the right subtree of the left child.
   - **Solution**: Apply an LR Rotation to rebalance.
2. **Right-Left (RL) Rotation**:
   - **Condition**: A node `Z` has a right child `Y`, and `Y` has a left child `X`.
   - **Imbalance**: Insertion in the left subtree of the right child.
   - **Solution**: Apply an RL Rotation to rebalance.

These double rotations address the zig-zag imbalance patterns that single rotations cannot fix, ensuring the tree remains balanced and operations remain efficient.

####  Why Use Tree Rotations?

Tree rotations are employed to:

- **Maintain Balance:** Prevent trees from becoming skewed (degenerate), ensuring operations remain efficient.
- **Restore Properties:** In self-balancing trees like AVL trees, red-black trees, and treaps, rotations help maintain specific invariants or properties after insertions and deletions.
- **Optimize Performance:** Balanced trees guarantee that operations such as search, insert, and delete have optimal time complexities (O(log n)).

