---
title: UI feedback [EN]
---

> For every action, there is an equal and opposite reaction.
>
> —Newton's Third Law

When a user performs an "action", such as clicking on a button, he expects the interfaz to respond with a "reaction". Otherwise the user might think that the action never took place.

That process (action - reaction) is what I generally refer to as "UI feedback". And I consider it a very important aspect in the development of user interfaces.

## And why is it so important to notify the user?

Let's imagine that we are withdrawing money from an ATM and suddenly the screen freezes. It'd be reasonable to think that something went wrong. But it could also mean that our request is still being processed. In that case, this stressful situation could avoided by displaying a loading icon.

## And how should we proceed?

We must keep in mind that things can go wrong, especially when dealing with external APIs. And it's our responsibility, as programmers, to notify the user of any event.

There are basically three types of events:

| Event   | Description            | Recommended action                       |
|---      |---                     |---                                       |
| Loading | The request is pending | Display a loading icon                   |
| Error   | The request failed     | Display an error message                 |
| Success | The request succeeded  | Display a success message <sup>(1)</sup> |

<sub>(1) Sometimes it's not necessary to show a successful message, since most of the time the application reflects the last operation. Don't overwhelm the user!</sub>

## A practical example

Let's have a look at this [CRUD application](https://github.com/gchumillas/crud) written in [React](https://reactjs.org/) and [TypeScript](https://www.typescriptlang.org/). In particular let's consider the following user interface (UI) and see what can we learn from it:

```jsx
// Create New Item (some parts have been snipped):
// https://github.com/gchumillas/crud/blob/master/src/routes/CreateItemDialog.tsx
// ...snip...

export default () => {
  // ...snip...

  const [state, onSubmit] = useAsyncFn(async () => {
    // The client didn't fill required fields.
    if (!title) {
      throw new Error('errors.requiredFields')
    }

    await createItem(token, title, description)
    refresh()
    history.push('/')
  }, [title, description])

  const status = _.get(state.error, 'response.status')
  const message = status
    ? `http.${status}`
    : _.get(state.error, 'message')

  // ...snip...

  return (
    <Dialog open fullWidth maxWidth="sm">
      <DialogTitle>{t('routes.createItem.title')}</DialogTitle>
      <DialogContent>
        <TextField autoFocus required label={t('routes.createItem.titleField')} value={title} onChange={setTitle} />
        <TextArea label={t('routes.createItem.descriptionField')} value={description} onChange={setDescription} />
      </DialogContent>
      {/* [ERROR]: Display a descriptive message */}
      {state.error && (
        <DialogContent>
          <DialogContentText color="error">
            {t(message)}
          </DialogContentText>
        </DialogContent>
      )}
      <DialogActions>
        <Button disabled={state.loading} onClick={() => history.push('/')}>{t('buttons.cancel')}</Button>
        {/* [LOADING]: Disable the submit button. */}
        <SubmitButton disabled={state.loading} onClick={onSubmit}>
          {t('buttons.continue')}
        </SubmitButton>
      </DialogActions>
    </Dialog>
  )
}
```

**Disable the submit button while the request is still pending**. This is a custom button that also shows a loading icon:

```jsx
{
    // ...
    <SubmitButton disabled={state.loading} onClick={onSubmit}>
      {t('buttons.continue')}
    </SubmitButton>
    // ...
}
```

**Show a message if an error has occurred**. Note that we do not write the message directly. Instead we use the [i18next.t](https://www.i18next.com/overview/api#t) function to translate the message to the appropriate language:

```jsx
{
  // ...
  {state.error && (
    <DialogContent>
      <DialogContentText color="error">
        {t(message)}
      </DialogContentText>
    </DialogContent>
  )}
  // ...  
}
```

**Not all errors come from the server-side.** The user can also generate errors. And those errors are obtained later from the `state.error` property:

```jsx
// The client didn't fill required fields.
if (!title) {
  throw new Error('errors.requiredFields')
}
```

**When an error occurs on the server-side, it is saved in the 'error.response' property.** Note that this property does not always exist. That's why we use the [`_.get`](https://lodash.com/docs/4.17.15#get) function:

```jsx
const status = _.get(state.error, 'response.status')
const message = status
  ? `http.${status}`
  : _.get(state.error, 'message')
```

**We obtain the server-side error from 'response.status', not from 'response.message'**, since this property usually only contains debug information:

```jsx
// WRONG !!!
const message = _.get(state.error, 'response.message')

// GOOD !!!
const status = _.get(state.error, 'response.status')
```

**Do not write literal messages. Use a translation system instead.** In our case we use the [i18next](https://www.i18next.com/) library:

```jsx
// WRONG !!!
throw new Error('Introduzca los campos requeridos.')

// GOOD !!!
throw new Error('errors.requiredFields')
```

## Some useful libraries

In the previous example we have used some useful libraries:

1. [Material-UI](https://github.com/mui-org/material-ui): a collection of visual components.
2. [react-i18next](https://github.com/i18next/react-i18next): a translation system.
3. [react-use](https://github.com/streamich/react-use): a collection of React hooks.
4. [Lodash](https://lodash.com): a library that expands the capabilities of JavaScript.

And that's all folks!
