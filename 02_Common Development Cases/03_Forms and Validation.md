# Forms and Validation

## Handling Form State
### Controlled vs Uncontrolled Inputs

Controlled components in React manage form state through state variables, while uncontrolled components use refs. Controlled inputs are more predictable and easier to validate.

**Demo TypeScript Code:**

```tsx
// Controlled vs Uncontrolled Form Inputs

// Controlled Form Component
const ControlledForm: React.FC = () => {
  const [value, setValue] = useState<string>('');

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setValue(event.target.value);
  };

  return (
    <div>
      <h2>Controlled Input</h2>
      <input
        type="text"
        value={value}
        onChange={handleChange}
        placeholder="Controlled input"
      />
      <p>Current Value: {value}</p>
    </div>
  );
};

// Uncontrolled Form Component
const UncontrolledForm: React.FC = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  const handleSubmit = () => {
    if (inputRef.current) {
      alert(`Uncontrolled input value: ${inputRef.current.value}`);
    }
  };

  return (
    <div>
      <h2>Uncontrolled Input</h2>
      <input
        type="text"
        ref={inputRef}
        placeholder="Uncontrolled input"
      />
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
};

// App Component
const App: React.FC = () => {
  return (
    <div>
      <ControlledForm />
      <UncontrolledForm />
    </div>
  );
};
```

**Comments in Code:**
- **`ControlledForm`**: Manages form state with `useState` and updates through `handleChange`.
- **`UncontrolledForm`**: Uses `useRef` to access the input value directly.

This demonstrates the difference between controlled and uncontrolled components in handling form state.



## Form Submission
### 1. Handling Form Events

Form submission in React involves handling form events to process or validate user input. Use event handlers to manage form submission and prevent default browser actions.

**Demo TypeScript Code:**

```tsx
// Form Component
const FormComponent: React.FC = () => {
  const [inputValue, setInputValue] = useState<string>('');
  const [submittedValue, setSubmittedValue] = useState<string>('');

  // Handle input change
  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setInputValue(event.target.value);
  };

  // Handle form submission
  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault(); // Prevents the default form submission action
    setSubmittedValue(inputValue); // Set submitted value
    console.log('Form submitted with value:', inputValue);
  };

  return (
    <div>
      <h2>Form Submission</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={inputValue}
          onChange={handleChange}
          placeholder="Enter something"
        />
        <button type="submit">Submit</button>
      </form>
      {submittedValue && <p>Submitted Value: {submittedValue}</p>}
    </div>
  );
};
```

**Comments in Code:**
- **`handleChange`**: Updates the `inputValue` state when the input changes.
- **`handleSubmit`**: Prevents the default form submission, processes the input value, and logs it.
- **Form Handling**: Demonstrates how to manage form events and submission in React.

### 2. Managing Form State

Managing form state in React involves tracking and updating form values through state. Use hooks to manage state and handle form submission.

**Demo TypeScript Code:**

```tsx
// Form Component
const FormComponent: React.FC = () => {
  const [formState, setFormState] = useState<{ name: string; email: string }>({
    name: '',
    email: '',
  });
  const [submitted, setSubmitted] = useState<boolean>(false);

  // Handle input changes
  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;
    setFormState(prevState => ({
      ...prevState,
      [name]: value,
    }));
  };

  // Handle form submission
  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    setSubmitted(true);
    console.log('Form submitted:', formState);
  };

  return (
    <div>
      <h2>Manage Form State</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          name="name"
          value={formState.name}
          onChange={handleChange}
          placeholder="Name"
        />
        <input
          type="email"
          name="email"
          value={formState.email}
          onChange={handleChange}
          placeholder="Email"
        />
        <button type="submit">Submit</button>
      </form>
      {submitted && (
        <div>
          <h3>Form Data:</h3>
          <p>Name: {formState.name}</p>
          <p>Email: {formState.email}</p>
        </div>
      )}
    </div>
  );
};
```

**Comments in Code:**
- **`handleChange`**: Updates `formState` based on input changes.
- **`handleSubmit`**: Handles form submission and logs `formState`.
- **Form Management**: Demonstrates state handling and form submission in React.



## Form Validation
### 1. Built-in Validation

**React Form Validation with Built-in Validation:**

Using HTML5 built-in validation features, such as `required`, `minLength`, and `pattern`, simplifies client-side validation.

**Demo TypeScript Code:**

```tsx
// Form Component with Built-in Validation
const ValidatedForm: React.FC = () => {
  // Handle form submission
  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    // Form will be validated automatically by browser
    console.log('Form submitted');
  };

  return (
    <div>
      <h2>Form Validation</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          name="username"
          placeholder="Username"
          required
          minLength={4}
          pattern="[A-Za-z]{4,}"
          title="Username must be at least 4 letters"
        />
        <input
          type="email"
          name="email"
          placeholder="Email"
          required
        />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
};
```

**Comments in Code:**
- **`required`**: Makes the field mandatory.
- **`minLength`**: Ensures the input has a minimum length.
- **`pattern`**: Validates input against a regex pattern.
- **`title`**: Provides a tooltip for invalid inputs.

### 2. Custom Validation Logic

**React Form Validation with Custom Validation Logic:**

Custom validation allows you to implement complex rules and error handling beyond HTML5 built-in validation.

**Demo TypeScript Code:**

```tsx
// Custom validation logic
const validateEmail = (email: string) => /\S+@\S+\.\S+/.test(email);

const CustomValidatedForm: React.FC = () => {
  const [email, setEmail] = useState<string>('');
  const [error, setError] = useState<string>('');

  // Handle form submission
  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    if (!validateEmail(email)) {
      setError('Invalid email address');
    } else {
      setError('');
      console.log('Form submitted with email:', email);
    }
  };

  return (
    <div>
      <h2>Custom Form Validation</h2>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          name="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
        <button type="submit">Submit</button>
        {error && <p style={{ color: 'red' }}>{error}</p>}
      </form>
    </div>
  );
};
```

**Comments in Code:**
- **`validateEmail`**: Custom function to check email validity.
- **`setError`**: Updates the error state if validation fails.
- **`onChange`**: Updates state with the input value.
- **`error`**: Displays validation error if any.



## Using Libraries (Formik, React Hook Form)
### Handling Errors and Messages

**React Form Handling with Libraries (Formik, React Hook Form):**

Use Formik or React Hook Form for streamlined form management, including error handling and validation messages.

**Formik Example with TypeScript:**

```tsx
import React from 'react';
import { Formik, Field, Form, ErrorMessage } from 'formik';
import * as Yup from 'yup';

// Validation schema with Yup
const validationSchema = Yup.object({
  email: Yup.string().email('Invalid email format').required('Required'),
});

const FormikForm: React.FC = () => (
  <Formik
    initialValues={{ email: '' }}
    validationSchema={validationSchema}
    onSubmit={(values) => {
      console.log('Form submitted with:', values);
    }}
  >
    <Form>
      <div>
        <label htmlFor="email">Email:</label>
        <Field type="text" id="email" name="email" />
        <ErrorMessage name="email" component="div" style={{ color: 'red' }} />
      </div>
      <button type="submit">Submit</button>
    </Form>
  </Formik>
);

export default FormikForm;
```

**React Hook Form Example with TypeScript:**

```tsx
import React from 'react';
import { useForm } from 'react-hook-form';

interface FormValues {
  email: string;
}

const ReactHookForm: React.FC = () => {
  const { register, handleSubmit, formState: { errors } } = useForm<FormValues>();

  const onSubmit = (data: FormValues) => {
    console.log('Form submitted with:', data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="text"
          id="email"
          {...register('email', { required: 'Email is required', pattern: { value: /\S+@\S+\.\S+/, message: 'Invalid email format' } })}
        />
        {errors.email && <p style={{ color: 'red' }}>{errors.email.message}</p>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default ReactHookForm;
```

**Comments in Code:**
- **Formik**: Uses `Formik` for form management, `Yup` for validation schema.
- **React Hook Form**: Uses `useForm` hook for form handling and validation.