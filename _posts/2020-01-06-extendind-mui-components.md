---
title: Extending Material-UI components
---

This will be the first of a series of brief articles related to the web development. And in this particular case we'll talk about "extending" [Material-UI](https://material-ui.com/) components. Note that I wrote "extending" (double quoted) on purpose, since we'll actually use the "composition" approach, which consists of an alternative way to change or "extend" the default behavior of a component or class.

As a reference, we'll use the following project:<br>
[https://github.com/gchumillas/crud](https://github.com/gchumillas/crud)

It consists of a typical CRUD application written in [React](https://reactjs.org/) and [TypeScript](https://www.typescriptlang.org/). That is, an application to [C]reate, [R]ead, [U]pdate or [D]elete items. And despite its simplicity, it contains some interesting topics that we could address in the next articles.

Let's say that we want to create a custom "Select Field". There's already a `Select` component in Material-UI:<br>
[https://material-ui.com/components/selects/](https://material-ui.com/components/selects/)

But that component is not easy to use, since we have to use it in combination with other components, such as `FormControl`, `InputLabel`, `MenuItem`, etc. Instead, we would like to create a component similar to `TextField`. Something like this:

```jsx
<SelectField
  label="Language"
  value={language}
  onChange={setLanguage}
  options={% raw  %}{[
    { value: 'en', label: 'English' },
    { value: 'es', label: 'Español' }
  ]}{% endraw  %}
/>
```

That is, we'd like to be able to declare the component in a single line. But, on the other hand, **we want to preserve the original properties of the `Select` component**. How could we achieve that in a simple way? Here it is:

```jsx
// src/components/fields/SelectField.tsx

import React from 'react'
import {
  Select, SelectProps,
  FormControl, InputLabel, MenuItem
} from '@material-ui/core'

type Props = Omit<SelectProps, 'onChange'> & {
  label?: string,
  options: { label: string, value: string }[],
  onChange?: (value: string) => void
}

export default ({
  label, value, options, onChange, fullWidth = true, ...rest
}: Props) => (
  <FormControl fullWidth={fullWidth}>
    {label && <InputLabel>{label}</InputLabel>}
    <Select
      value={value}
      onChange={e => onChange && onChange(`${e.target.value}`)}
      {...rest}>
      {options.map(({ value, label }, key) => (
        <MenuItem key={key} value={value}>{label}</MenuItem>
      ))}
    </Select>
  </FormControl>
)
```

In the previous example we used `SelectProps` as the base type. And we added the `label` and `options` properties. We also replaced the `onChange` property by a different one. Note the use of the [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittk) utility, the [intersection type](https://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types) (&) and the [{...rest}](https://www.typescriptlang.org/docs/handbook/functions.html#rest-parameters) parameter.

And that's all, folks! You'll find more examples in the next folders:<br>
[https://github.com/gchumillas/crud/tree/master/src/components/fields](https://github.com/gchumillas/crud/tree/master/src/components/fields)
[https://github.com/gchumillas/crud/tree/master/src/components/buttons](https://github.com/gchumillas/crud/tree/master/src/components/buttons)
