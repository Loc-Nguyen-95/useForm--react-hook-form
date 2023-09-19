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
## First step: `Register feild`

## Apply validation 
