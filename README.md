# Docs
**Intallation in one line** 😛

  npm install react-hook-form

## Example 
```swift
import { useForm } from "react-hook-form"

export default function App() {
  const {
    register,
    handleSubmit,
    watch,
    formState: { errors },
  } = useForm()

  const onSubmit = (data) => console.log(data)

  console.log(watch("example"))

  return (
   
    <form onSubmit={handleSubmit(onSubmit)}>
      
      <input defaultValue="test" {...register("example")} />

      <input {...register("exampleRequired", { required: true })} />
    
      {errors.exampleRequired && <span>This field is required</span>}

      <input type="submit" />
    </form>
  )
}
```
## `Feature 1`: Register feild
Register là một trong những chìa khoá quan trọng để đăng kí component vào hook 
Làm cho value có gía trị cho cả validation và submission 
Mỗi field required key cho registration 

```swift
import { useForm } from "react-hook-form"

export default function App() {
  const { register, handleSubmit } = useForm()
  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>

      <input {...register("firstName")} />
      <select {...register("gender")}>

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

Hook làm cho validation dễ dàng hơn bằng cách sắp xếp HTML standard cho form validation
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
  const { register, handleSubmit } = useForm()
  const onSubmit = (data) => console.log(data)

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName", { required: true, maxLength: 20 })} />
      <input {...register("lastName", { pattern: /^[A-Za-z]+$/i })} />
      <input type="number" {...register("age", { min: 18, max: 99 })} />
      <input type="submit" />
    </form>
  )
}
```
## Feature 3: Interage - Tích hợp vào Form sẵn có

The important step is to register the **component's ref** and assign **relevant props** to your input.

```swift
import { useForm } from "react-hook-form"

// The following component is an example of your existing Input Component
const Input = ({ label, register, required }) => (
  <>
    <label>{label}</label>
    <input {...register(label, { required })} />
  </>
)

// you can use React.forwardRef to pass the ref too
const Select = React.forwardRef(({ onChange, onBlur, name, label }, ref) => (
  <>
    <label>{label}</label>
    <select name={name} ref={ref} onChange={onChange} onBlur={onBlur}>
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


## Feature 4 : Tích hợp với UI libraries 

## Feature 5: Tích hợp vào Controlled Input

## Feature 6:Tích hợp với Globel State

## Feature 7: Xử lí lỗi

## Feature 8: Tích hợp với services 

## Feature 9: Schema


