---
title: Extending MUI components [EN]
---

This will be the first of a serie of brief articles related to the web development. And in this particular case we'll talk about "extending" [Material-UI](https://material-ui.com/) components. Note that I wrote "extending" (double quoted) on purpose, since we'll actually use the "composition" approach, which consists on an alternative way to change or "extend" the default behavior of a component or class.
<!-- TODO: talk about the advantages of using composition over inheritance -->

To achieve or goal we'll use the following project:<br>
[https://github.com/gchumillas/crud](https://github.com/gchumillas/crud)

<!-- TODO: do not repeat "it consists" -->
It consists on a typical CRUD application written in [TypeScript](https://www.typescriptlang.org/). That is, an application to [C]reate, [R]ead, [U]pdate or [D]elete items. And despite its simplicity, it contains several interesting aspects that we could address.

Let's say that we want to create a custom "Select Field". There's already a `Select` component in Material-UI:<br>
[https://material-ui.com/components/selects/](https://material-ui.com/components/selects/)

But that component is not easy to use, since we have to use it in combination with other components, such us `FormControl`, `InputLabel`, `MenuItem`, etc. Instead, we would like to create a component similar to `TextField`. Something like this:

```jsx
<SelectField
  label="Language"
  value={language}
  options={% raw  %} {{ value: 'en', label: 'English', value: 'es', label: 'Español' }} {% endraw  %}
  onChange={value => setLanguage(value)}
/>
```

That is, we would like to declare the component in a single line. But, on the other hand, we'd like to preserve the original properties of the `Select` component. How could we achieve that in a simple way? That is!

```jsx
import React from 'react'
import { Select, SelectProps, FormControl, InputLabel, MenuItem } from '@material-ui/core'

type Props = Omit<SelectProps, 'onChange'> & {
  label?: string,
  options: { label: string, value: string }[],
  onChange?: (value: string) => void
}

export default ({ label, value, options, onChange }: Props) => (
  <FormControl fullWidth>
    {label && <InputLabel>{label}</InputLabel>}
    <Select value={value} onChange={e => onChange && onChange(`${e.target.value}`)}>
      {options.map(({ value, label }, key) => (
        <MenuItem key={key} value={value}>{label}</MenuItem>
      ))}
    </Select>
  </FormControl>
)
```

In the previous example we used `SelectProps` as the base type. And we added the `label` and `options` properties. We also replaced the `onChange` property by a different one. Note the use of [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittk) and [&](https://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types).

And that's all, folks! You'll find more examples in the next folder:<br>
[https://github.com/gchumillas/crud/tree/master/src/components/fields](https://github.com/gchumillas/crud/tree/master/src/components/fields)
