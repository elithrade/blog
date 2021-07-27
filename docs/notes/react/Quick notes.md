# React quick notes

## `useCallback` with no dependencies

The following function is wrapped in `useCallback`, but it doesn't have any dependencies.
```javascript
const updateChildren = useCallback(
  (id: string, item: ITreeItem, children: ITreeItem[]) => {
    if (item.id === id) {
      item.children.clear();
      children.forEach((child) => item.children.set(child.id, child));
      return;
    }
    for (const [, value] of item.children) {
      updateChildren(id, value, children);
    }
  },
  []
);
```
* Purpose of `useCallback` does not depend on if you have dependencies or not. It's to ensure referential integrity. To get better performance. Every time your component re-render, a new instance of the function is created, `useCallback` is just an addition which assigns the reference to another variable. 
* When you wrap a function with useCallback it actually memorizing(remember the last execution result) and the next time you call this function it will return the last result if none of the values in the dependent array is changed.