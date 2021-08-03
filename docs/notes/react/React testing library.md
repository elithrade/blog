# React testing library

## Always use Queries when query for elements in DOM

I wrote a test using [`react-testing-library`](https://testing-library.com/docs/react-testing-library/intro) and [`msw`](https://mswjs.io/) to test expand and collapse a tree. It fires 2 click events, the first will expand the tree root node and the second will collapse it.

```js
it('toggles node expansion states when clicking', async () => {
  renderFolderTree();
  setupFolderData();

  const rootFolder = await screen.findByText('root');

  expect(screen.queryByText('child1')).not.toBeInTheDocument();
  expect(screen.queryByText('child2')).not.toBeInTheDocument();

  setupFolderData();

  fireEvent.click(rootFolder);

  // Wait for async data fetching.
  await waitFor(() => screen.getByText('child1'));

  expect(screen.getByText('child1')).toBeInTheDocument();
  expect(screen.getByText('child2')).toBeInTheDocument();

  fireEvent.click(rootFolder);

  expect(screen.getByText('child1')).not.toBeInTheDocument();
  expect(screen.getByText('child2')).not.toBeInTheDocument();
});
```

The test failed because `child1` and `child2` are still in DOM after the second click.
It turns out to be that the event for seconds click on `rootFolder` element never triggered the actual clicked event handler in the tree component.

This is fixed by always using Queries when querying elements in the DOM.

```js
  fireEvent.click(screen.getByText('root'));
```

The lesson learnt is do not keep local reference of queried element, instead alway use Queries, the reference of already queried elements may go out of scope and `fireEvent` will not trigger the event handlers.