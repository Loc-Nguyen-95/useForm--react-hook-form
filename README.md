# Docs
**Intallation in one line** 😛

  npm install react-hook-form

## Example 
```swift
import { useForm } from "react-hook-form" // hook

export default function App() {
  const {
    register,
    handleSubmit,
    watch, //?
    formState: { errors },
  } = useForm()

  const onSubmit = (data) => console.log(data)

  console.log(watch("example"))

  return (
   
    <form onSubmit={ handleSubmit(onSubmit) }>
      <input
        defaultValue="test"
        {...register("example")} //key -> watch 
      />
      <input
          {...register("exampleRequired", { required: true })} // apply validation
        />
      {errors.exampleRequired && <span>This field is required</span>} // validation message 
      <input type="submit" />
    </form>
  )
}
```
## `Feature 1`: Register feild
Register là một trong những chìa khoá quan trọng để đăng kí component vào hook 
Làm cho **value** có gía trị cho cả validation và submission 
Mỗi field required **key** cho **registration** 

```swift
import { useForm } from "react-hook-form"

export default function App() {
  const {
    register,
    handleSubmit
  } = useForm();

  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register("firstName")}
      />
      <select
        {...register("gender")}
        >
          <option value="female">female</option>
          <option value="male">male</option>
          <option value="other">other</option>
      </select>
      <input type="submit" />
    </form>
  )
}
```

## Feature 2: Apply validation 

Hook làm cho validation dễ dàng hơn bằng cách sắp xếp **HTML standard** cho form validation
List of rules 
- required
- min
- max
- minLength
- maxLength
- pattern
- validate

```swift
import { useForm } from "react-hook-form"

export default function App() {
  const {
    register,
    handleSubmit
  } = useForm();

  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register("firstName", { required: true, maxLength: 20 })}
      />
      <input
        {...register("lastName", { pattern: /^[A-Za-z]+$/i })}
      />
      <input
        type="number" {...register("age", { min: 18, max: 99 })}
      />
      <input type="submit" />
    </form>
  )
}
```
## `Feature 3`: Interage - Tích hợp vào Form sẵn có !!

Tích hợp vào Form sẵn có 1 cách dễ dàng. Bước quan trọng là rigister component's ref và gán props tương ứng vào input
(Viết component Input)

```swift
import { useForm } from "react-hook-form"

//  Register input với {label}
const Input = ({ label, register, required }) => (
  <>
    <label>{label}</label>
    <input {...register(label, { required })} />
  </>
)

// sử dụng React.forwardRef để truyền ref

const Select = React.forwardRef( ( { onChange, onBlur, name, label }, ref) => (
  <>
    <label>{label}</label>
    <select ref={ref} name={name} onChange={onChange} onBlur={onBlur}>
      <option value="20">20</option>
      <option value="30">30</option>
    </select>
  </>
))

const App = () => {
  const { register, handleSubmit } = useForm()

  const onSubmit = (data) => {
    alert(JSON.stringify(data))
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <Input label="First Name" register={register} required />

      <Select label="Age" {...register("Age")} />

      <input type="submit" />
    </form>
  )
}
```

## `Feature 4` : Tích hợp với `UI libraries` !!
React hook form cho phép tích hợp với UI component libraries một cách dễ dàng. Nếu component không có **input's ref**, nên sử dụng Controller component -> dễ dàng cho xử lí quá trình registration 
```swift
import { useForm, Controller } from "react-hook-form"
import Select from "react-select"
import Input from "@material-ui/core/Input"

const App = () => {
  const {
    control,
    handleSubmit
  } = useForm({
    defaultValues: {
      firstName: "",
      select: {},
    }, 
  })

  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>

      <Controller
        name="firstName"
        control={control}
        render={ ( {field} ) => <Input {...field} />}
      />

      <Controller
        name="select"
        control={control}
        render={ ({ field }) => (
          <Select
            {...field}
            options={[
              { value: "chocolate", label: "Chocolate" },
              { value: "strawberry", label: "Strawberry" },
              { value: "vanilla", label: "Vanilla" },
            ]}
          />

        )}
      />
      <input type="submit" />
    </form>
  )
}
```
## `Feature 5`: Tích hợp vào `Controlled Input` !!
#### useController()
Thư viện đi với Uncontrolled component và HTML input nguyên bản, tuy nhiên không tránh gặp phải những component như React-Select, AntD, MUI ... 
Để làm mọi thứ dễ dàng hơn, chúng tôi cung cấp wrappeer component: Controller, để sắp xếp hợp lí quá trình tích hợp trong khi vẫn cho phép tự do sử dụng register 
```swift
import { TextField } from "@material-ui/core";

import { useController } from "react-hook-form"

//
function Input({ control, name }) {
  const {
    field,
    fieldState: { invalid, isTouched, isDirty },
    formState: { touchedFields, dirtyFields },
  } = useController( {
    name,
    control,
    rules: { required: true },
  } )

  return (
    <TextField
      onChange={field.onChange} // send value to hook form
      onBlur={field.onBlur} // notify when input is touched/blur
      value={field.value} // input value
      name={field.name} // send down the input name
      inputRef={field.ref} // send input ref, so we can focus on input when error appear
    />
  )
}
```
## Feature 6:Tích hợp với Global State
#### Redux state 
Thư viện không yêu cầu dựa vào những thư viện quản lí state khác nhưng bạn có thể dễ dàn tích hợp với chúng 

```swift
import { useForm } from "react-hook-form"
import { connect } from "react-redux"
import updateAction from "./actions"

export default function App(props) {
  const { register, handleSubmit, setValue } = useForm({
    defaultValues: {
      firstName: "",
      lastName: "",
    },
  })
  // Submit your data into Redux store
  const onSubmit = (data) => props.updateAction(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} />
      <input {...register("lastName")} />
      <input type="submit" />
    </form>
  )
}

// Connect your component with redux
connect(
  ({ firstName, lastName }) => ({ firstName, lastName }),
  updateAction
)(YourForm)
```
## Feature 7: Xử lí lỗi
Thư viện cung cấp errors object để hiển thị lỗi
```swift
import { useForm } from "react-hook-form"

export default function App() {
  const {
    register,
    formState: { errors },
    handleSubmit,
  } = useForm();

  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        {...register("firstName", { required: true })}
        aria-invalid={errors.firstName ? "true" : "false"}
      />
      {errors.firstName?.type === "required" && (
        <p role="alert">First name is required</p>
      )}

      <input
        {...register("mail", { required: "Email Address is required" })}
        aria-invalid={errors.mail ? "true" : "false"}
      />
      {errors.mail && <p role="alert">{errors.mail.message}</p>}

      <input type="submit" />
    </form>
  )
}
```
## Feature 8: Tích hợp với services  (server)
#### Form -> action
```swift
import { Form } from "react-hook-form"

function App() {
  const { register, control } = useForm()

  return (
    <Form
      action="/api/save"
        // Send post request with the FormData
        // encType={'application/json'} you can also switch to json object
      onSuccess={ () => {
        alert("Your application is updated.")
      }}
      onError={() => {
        alert("Submission has failed.")
      }}
      control={control}
    >
      <input {...register("firstName", { required: true })} />
      <input {...register("lastName", { required: true })} />
      <button>Submit</button>
    </Form>
  )
}
```
## Feature 9: Schema
### Mô hình sẵn cho validation (VD: yup)
Return errors hay valid result 
```swift
import { useForm } from "react-hook-form"
import { yupResolver } from "@hookform/resolvers/yup"
import * as yup from "yup"

const schema = yup
  .object({
    firstName: yup.string().required(),
    age: yup.number().positive().integer().required(),
  })
  .required()

export default function App() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: yupResolver(schema),
  })
  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} />
      <p>{errors.firstName?.message}</p>

      <input {...register("age")} />
      <p>{errors.age?.message}</p>

      <input type="submit" />
    </form>
  )
}
```

