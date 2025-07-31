# Components Documentation

## Overview

This document provides comprehensive documentation for all reusable UI components and their usage.

## Table of Contents

- [Form Components](#form-components)
- [Navigation Components](#navigation-components)
- [Layout Components](#layout-components)
- [Data Display Components](#data-display-components)
- [Feedback Components](#feedback-components)
- [Input Components](#input-components)
- [Media Components](#media-components)
- [Utility Components](#utility-components)

## Form Components

### `<Form>`

A flexible form component with built-in validation and submission handling.

**Props:**
- `onSubmit` (function, required): Form submission handler
- `validationSchema` (object, optional): Validation schema
- `initialValues` (object, optional): Initial form values
- `children` (ReactNode): Form fields and elements
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Form, Input, Button } from './components';

function LoginForm() {
  const handleSubmit = (values) => {
    console.log('Form values:', values);
  };

  const validationSchema = {
    email: { required: true, type: 'email' },
    password: { required: true, minLength: 8 }
  };

  return (
    <Form 
      onSubmit={handleSubmit}
      validationSchema={validationSchema}
      initialValues={{ email: '', password: '' }}
    >
      <Input name="email" label="Email" type="email" />
      <Input name="password" label="Password" type="password" />
      <Button type="submit">Login</Button>
    </Form>
  );
}
```

### `<Input>`

Reusable input field with validation and error display.

**Props:**
- `name` (string, required): Field name
- `label` (string, optional): Input label
- `type` (string, optional): Input type (default: 'text')
- `placeholder` (string, optional): Placeholder text
- `required` (boolean, optional): Whether field is required
- `disabled` (boolean, optional): Whether input is disabled
- `error` (string, optional): Error message to display
- `onChange` (function, optional): Change handler
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Input } from './components';

function UserForm() {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');

  return (
    <div>
      <Input
        name="email"
        label="Email Address"
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        error={error}
        placeholder="Enter your email"
        required
      />
    </div>
  );
}
```

### `<Select>`

Dropdown select component with search and multi-select support.

**Props:**
- `options` (array, required): Array of option objects
- `value` (any, optional): Selected value(s)
- `onChange` (function, required): Selection change handler
- `placeholder` (string, optional): Placeholder text
- `searchable` (boolean, optional): Enable search functionality
- `multiple` (boolean, optional): Allow multiple selections
- `disabled` (boolean, optional): Whether select is disabled
- `clearable` (boolean, optional): Show clear button

**Usage:**
```jsx
import { Select } from './components';

function CountrySelector() {
  const [selectedCountry, setSelectedCountry] = useState('');
  
  const countries = [
    { value: 'us', label: 'United States' },
    { value: 'ca', label: 'Canada' },
    { value: 'uk', label: 'United Kingdom' }
  ];

  return (
    <Select
      options={countries}
      value={selectedCountry}
      onChange={setSelectedCountry}
      placeholder="Select a country"
      searchable
      clearable
    />
  );
}
```

## Navigation Components

### `<Navigation>`

Main navigation component with responsive menu support.

**Props:**
- `items` (array, required): Navigation items
- `logo` (ReactNode, optional): Logo element
- `variant` (string, optional): 'horizontal' | 'vertical' (default: 'horizontal')
- `sticky` (boolean, optional): Whether navigation sticks to top
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Navigation } from './components';

function App() {
  const navItems = [
    { label: 'Home', href: '/', active: true },
    { label: 'About', href: '/about' },
    { label: 'Contact', href: '/contact' },
    {
      label: 'Services',
      children: [
        { label: 'Web Design', href: '/services/web-design' },
        { label: 'Development', href: '/services/development' }
      ]
    }
  ];

  return (
    <Navigation
      items={navItems}
      logo={<img src="/logo.png" alt="Company Logo" />}
      sticky
    />
  );
}
```

### `<Breadcrumb>`

Breadcrumb navigation showing current page hierarchy.

**Props:**
- `items` (array, required): Breadcrumb items with label and href
- `separator` (ReactNode, optional): Custom separator element
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Breadcrumb } from './components';

function ProductPage() {
  const breadcrumbItems = [
    { label: 'Home', href: '/' },
    { label: 'Products', href: '/products' },
    { label: 'Electronics', href: '/products/electronics' },
    { label: 'iPhone 15', href: '/products/electronics/iphone-15' }
  ];

  return (
    <div>
      <Breadcrumb items={breadcrumbItems} />
      <h1>iPhone 15</h1>
    </div>
  );
}
```

### `<Pagination>`

Pagination component for navigating through pages of content.

**Props:**
- `currentPage` (number, required): Current active page
- `totalPages` (number, required): Total number of pages
- `onPageChange` (function, required): Page change handler
- `showFirstLast` (boolean, optional): Show first/last buttons
- `showPrevNext` (boolean, optional): Show previous/next buttons
- `maxVisiblePages` (number, optional): Maximum visible page numbers

**Usage:**
```jsx
import { Pagination } from './components';

function ProductList() {
  const [currentPage, setCurrentPage] = useState(1);
  const totalPages = 10;

  return (
    <div>
      {/* Product list content */}
      <Pagination
        currentPage={currentPage}
        totalPages={totalPages}
        onPageChange={setCurrentPage}
        showFirstLast
        maxVisiblePages={5}
      />
    </div>
  );
}
```

## Layout Components

### `<Container>`

Responsive container component with max-width constraints.

**Props:**
- `size` (string, optional): 'sm' | 'md' | 'lg' | 'xl' | 'full' (default: 'lg')
- `padding` (boolean, optional): Apply default padding (default: true)
- `center` (boolean, optional): Center content horizontally (default: true)
- `children` (ReactNode): Content to wrap
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Container } from './components';

function HomePage() {
  return (
    <Container size="lg" padding center>
      <h1>Welcome to our website</h1>
      <p>This content is centered and has responsive width.</p>
    </Container>
  );
}
```

### `<Grid>`

Flexible grid system for layout organization.

**Props:**
- `columns` (number | object, optional): Number of columns or responsive columns
- `gap` (string | number, optional): Gap between grid items
- `align` (string, optional): Vertical alignment
- `justify` (string, optional): Horizontal alignment
- `children` (ReactNode): Grid items
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Grid, GridItem } from './components';

function ProductGrid() {
  return (
    <Grid columns={{ sm: 1, md: 2, lg: 3 }} gap="1rem">
      <GridItem>
        <ProductCard title="Product 1" />
      </GridItem>
      <GridItem>
        <ProductCard title="Product 2" />
      </GridItem>
      <GridItem>
        <ProductCard title="Product 3" />
      </GridItem>
    </Grid>
  );
}
```

### `<Sidebar>`

Collapsible sidebar component for navigation or content.

**Props:**
- `isOpen` (boolean, required): Whether sidebar is open
- `onToggle` (function, required): Toggle handler
- `position` (string, optional): 'left' | 'right' (default: 'left')
- `width` (string, optional): Sidebar width (default: '250px')
- `overlay` (boolean, optional): Show overlay when open on mobile
- `children` (ReactNode): Sidebar content

**Usage:**
```jsx
import { Sidebar, Button } from './components';

function Layout() {
  const [sidebarOpen, setSidebarOpen] = useState(false);

  return (
    <div>
      <Button onClick={() => setSidebarOpen(!sidebarOpen)}>
        Toggle Sidebar
      </Button>
      
      <Sidebar
        isOpen={sidebarOpen}
        onToggle={setSidebarOpen}
        position="left"
        overlay
      >
        <nav>
          <ul>
            <li><a href="/dashboard">Dashboard</a></li>
            <li><a href="/profile">Profile</a></li>
            <li><a href="/settings">Settings</a></li>
          </ul>
        </nav>
      </Sidebar>
    </div>
  );
}
```

## Data Display Components

### `<Table>`

Feature-rich table component with sorting, filtering, and pagination.

**Props:**
- `data` (array, required): Array of data objects
- `columns` (array, required): Column definitions
- `sortable` (boolean, optional): Enable sorting (default: true)
- `filterable` (boolean, optional): Enable filtering (default: false)
- `pagination` (object, optional): Pagination settings
- `loading` (boolean, optional): Show loading state
- `onRowClick` (function, optional): Row click handler

**Usage:**
```jsx
import { Table } from './components';

function UserTable() {
  const users = [
    { id: 1, name: 'John Doe', email: 'john@example.com', status: 'active' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', status: 'inactive' }
  ];

  const columns = [
    { key: 'name', label: 'Name', sortable: true },
    { key: 'email', label: 'Email', sortable: true },
    { 
      key: 'status', 
      label: 'Status',
      render: (value) => (
        <span className={`status status-${value}`}>
          {value}
        </span>
      )
    },
    {
      key: 'actions',
      label: 'Actions',
      render: (_, row) => (
        <Button onClick={() => editUser(row.id)}>
          Edit
        </Button>
      )
    }
  ];

  return (
    <Table
      data={users}
      columns={columns}
      sortable
      pagination={{ pageSize: 10 }}
      onRowClick={(row) => console.log('Clicked:', row)}
    />
  );
}
```

### `<Card>`

Flexible card component for displaying content.

**Props:**
- `title` (string, optional): Card title
- `subtitle` (string, optional): Card subtitle
- `image` (string, optional): Header image URL
- `actions` (ReactNode, optional): Action buttons or elements
- `padding` (string, optional): Custom padding
- `shadow` (boolean, optional): Apply shadow effect
- `hoverable` (boolean, optional): Add hover effects
- `children` (ReactNode): Card content

**Usage:**
```jsx
import { Card, Button } from './components';

function ProductCard({ product }) {
  return (
    <Card
      title={product.name}
      subtitle={`$${product.price}`}
      image={product.imageUrl}
      shadow
      hoverable
      actions={
        <div>
          <Button variant="primary">Add to Cart</Button>
          <Button variant="secondary">View Details</Button>
        </div>
      }
    >
      <p>{product.description}</p>
      <div className="product-features">
        {product.features.map(feature => (
          <span key={feature} className="feature-tag">
            {feature}
          </span>
        ))}
      </div>
    </Card>
  );
}
```

### `<List>`

Customizable list component for displaying items.

**Props:**
- `items` (array, required): Array of list items
- `renderItem` (function, required): Function to render each item
- `divider` (boolean, optional): Show dividers between items
- `dense` (boolean, optional): Compact spacing
- `selectable` (boolean, optional): Allow item selection
- `selectedItems` (array, optional): Currently selected items
- `onSelect` (function, optional): Selection handler

**Usage:**
```jsx
import { List } from './components';

function NotificationList({ notifications }) {
  const renderNotification = (notification) => (
    <div className="notification-item">
      <div className="notification-content">
        <h4>{notification.title}</h4>
        <p>{notification.message}</p>
        <span className="notification-time">
          {formatDate(notification.createdAt)}
        </span>
      </div>
      {!notification.read && (
        <div className="notification-badge">New</div>
      )}
    </div>
  );

  return (
    <List
      items={notifications}
      renderItem={renderNotification}
      divider
      dense
    />
  );
}
```

## Feedback Components

### `<Alert>`

Alert component for displaying various types of messages.

**Props:**
- `type` (string, optional): 'success' | 'warning' | 'error' | 'info' (default: 'info')
- `title` (string, optional): Alert title
- `message` (string, required): Alert message
- `closable` (boolean, optional): Show close button
- `onClose` (function, optional): Close handler
- `icon` (ReactNode, optional): Custom icon
- `actions` (ReactNode, optional): Action buttons

**Usage:**
```jsx
import { Alert } from './components';

function FormSuccess() {
  const [showAlert, setShowAlert] = useState(true);

  if (!showAlert) return null;

  return (
    <Alert
      type="success"
      title="Success!"
      message="Your form has been submitted successfully."
      closable
      onClose={() => setShowAlert(false)}
      actions={
        <Button variant="primary" size="sm">
          View Details
        </Button>
      }
    />
  );
}
```

### `<Modal>`

Modal dialog component with overlay and focus management.

**Props:**
- `isOpen` (boolean, required): Whether modal is open
- `onClose` (function, required): Close handler
- `title` (string, optional): Modal title
- `size` (string, optional): 'sm' | 'md' | 'lg' | 'xl' (default: 'md')
- `closable` (boolean, optional): Show close button (default: true)
- `overlay` (boolean, optional): Show overlay (default: true)
- `center` (boolean, optional): Center modal vertically
- `children` (ReactNode): Modal content

**Usage:**
```jsx
import { Modal, Button } from './components';

function UserEditModal({ user, isOpen, onClose, onSave }) {
  const [formData, setFormData] = useState(user);

  const handleSave = () => {
    onSave(formData);
    onClose();
  };

  return (
    <Modal
      isOpen={isOpen}
      onClose={onClose}
      title="Edit User"
      size="md"
      center
    >
      <form>
        <Input
          label="Name"
          value={formData.name}
          onChange={(e) => setFormData({...formData, name: e.target.value})}
        />
        <Input
          label="Email"
          type="email"
          value={formData.email}
          onChange={(e) => setFormData({...formData, email: e.target.value})}
        />
        <div className="modal-actions">
          <Button variant="secondary" onClick={onClose}>
            Cancel
          </Button>
          <Button variant="primary" onClick={handleSave}>
            Save Changes
          </Button>
        </div>
      </form>
    </Modal>
  );
}
```

### `<Toast>`

Toast notification component for temporary messages.

**Props:**
- `message` (string, required): Toast message
- `type` (string, optional): 'success' | 'warning' | 'error' | 'info'
- `duration` (number, optional): Auto-dismiss duration in ms (default: 5000)
- `position` (string, optional): 'top-right' | 'top-left' | 'bottom-right' | 'bottom-left'
- `closable` (boolean, optional): Show close button
- `onClose` (function, optional): Close handler

**Usage:**
```jsx
import { Toast, useToast } from './components';

function App() {
  const { showToast } = useToast();

  const handleSave = async () => {
    try {
      await saveData();
      showToast({
        message: 'Data saved successfully!',
        type: 'success'
      });
    } catch (error) {
      showToast({
        message: 'Failed to save data',
        type: 'error'
      });
    }
  };

  return (
    <div>
      <Button onClick={handleSave}>Save Data</Button>
    </div>
  );
}
```

## Input Components

### `<Button>`

Versatile button component with multiple variants and states.

**Props:**
- `variant` (string, optional): 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'ghost'
- `size` (string, optional): 'sm' | 'md' | 'lg' (default: 'md')
- `disabled` (boolean, optional): Whether button is disabled
- `loading` (boolean, optional): Show loading state
- `fullWidth` (boolean, optional): Take full width of container
- `leftIcon` (ReactNode, optional): Icon before text
- `rightIcon` (ReactNode, optional): Icon after text
- `onClick` (function, optional): Click handler
- `type` (string, optional): Button type for forms
- `children` (ReactNode): Button content

**Usage:**
```jsx
import { Button, Icons } from './components';

function ActionButtons() {
  const [loading, setLoading] = useState(false);

  const handleSubmit = async () => {
    setLoading(true);
    await submitForm();
    setLoading(false);
  };

  return (
    <div className="button-group">
      <Button 
        variant="primary" 
        size="lg"
        onClick={handleSubmit}
        loading={loading}
        leftIcon={<Icons.Save />}
      >
        Save Changes
      </Button>
      
      <Button variant="secondary" size="md">
        Cancel
      </Button>
      
      <Button 
        variant="danger" 
        size="sm"
        rightIcon={<Icons.Trash />}
      >
        Delete
      </Button>
    </div>
  );
}
```

### `<Checkbox>`

Checkbox input with label and validation support.

**Props:**
- `checked` (boolean, optional): Whether checkbox is checked
- `onChange` (function, required): Change handler
- `label` (string, optional): Checkbox label
- `disabled` (boolean, optional): Whether checkbox is disabled
- `indeterminate` (boolean, optional): Indeterminate state
- `error` (string, optional): Error message
- `name` (string, optional): Input name
- `value` (string, optional): Input value

**Usage:**
```jsx
import { Checkbox } from './components';

function PreferencesForm() {
  const [preferences, setPreferences] = useState({
    notifications: true,
    newsletter: false,
    marketing: false
  });

  const handleChange = (name) => (checked) => {
    setPreferences(prev => ({
      ...prev,
      [name]: checked
    }));
  };

  return (
    <div>
      <Checkbox
        checked={preferences.notifications}
        onChange={handleChange('notifications')}
        label="Email notifications"
      />
      <Checkbox
        checked={preferences.newsletter}
        onChange={handleChange('newsletter')}
        label="Newsletter subscription"
      />
      <Checkbox
        checked={preferences.marketing}
        onChange={handleChange('marketing')}
        label="Marketing emails"
      />
    </div>
  );
}
```

### `<RadioGroup>`

Radio button group for single selection.

**Props:**
- `options` (array, required): Array of radio options
- `value` (string, optional): Selected value
- `onChange` (function, required): Change handler
- `name` (string, required): Group name
- `orientation` (string, optional): 'horizontal' | 'vertical' (default: 'vertical')
- `disabled` (boolean, optional): Whether group is disabled
- `error` (string, optional): Error message

**Usage:**
```jsx
import { RadioGroup } from './components';

function PaymentMethod() {
  const [paymentMethod, setPaymentMethod] = useState('card');

  const paymentOptions = [
    { value: 'card', label: 'Credit Card', description: 'Pay with credit or debit card' },
    { value: 'paypal', label: 'PayPal', description: 'Pay with your PayPal account' },
    { value: 'bank', label: 'Bank Transfer', description: 'Direct bank transfer' }
  ];

  return (
    <RadioGroup
      options={paymentOptions}
      value={paymentMethod}
      onChange={setPaymentMethod}
      name="payment-method"
      orientation="vertical"
    />
  );
}
```

## Media Components

### `<Image>`

Enhanced image component with lazy loading and fallbacks.

**Props:**
- `src` (string, required): Image source URL
- `alt` (string, required): Alt text for accessibility
- `width` (number | string, optional): Image width
- `height` (number | string, optional): Image height
- `lazy` (boolean, optional): Enable lazy loading (default: true)
- `fallback` (string, optional): Fallback image URL
- `placeholder` (ReactNode, optional): Loading placeholder
- `className` (string, optional): Additional CSS classes
- `onLoad` (function, optional): Load event handler
- `onError` (function, optional): Error event handler

**Usage:**
```jsx
import { Image } from './components';

function ProductImage({ product }) {
  return (
    <Image
      src={product.imageUrl}
      alt={product.name}
      width="300"
      height="200"
      lazy
      fallback="/images/placeholder.jpg"
      placeholder={<div className="image-skeleton" />}
      className="product-image"
    />
  );
}
```

### `<Avatar>`

User avatar component with initials fallback.

**Props:**
- `src` (string, optional): Avatar image URL
- `name` (string, optional): User name for initials
- `size` (string, optional): 'xs' | 'sm' | 'md' | 'lg' | 'xl' (default: 'md')
- `shape` (string, optional): 'circle' | 'square' (default: 'circle')
- `color` (string, optional): Background color for initials
- `onClick` (function, optional): Click handler
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Avatar } from './components';

function UserProfile({ user }) {
  return (
    <div className="user-profile">
      <Avatar
        src={user.avatarUrl}
        name={user.name}
        size="lg"
        onClick={() => openProfileModal(user)}
      />
      <div className="user-info">
        <h3>{user.name}</h3>
        <p>{user.email}</p>
      </div>
    </div>
  );
}
```

## Utility Components

### `<Loading>`

Loading spinner and skeleton components.

**Props:**
- `type` (string, optional): 'spinner' | 'skeleton' | 'dots' (default: 'spinner')
- `size` (string, optional): 'sm' | 'md' | 'lg' (default: 'md')
- `color` (string, optional): Loading indicator color
- `text` (string, optional): Loading text
- `overlay` (boolean, optional): Show as overlay
- `className` (string, optional): Additional CSS classes

**Usage:**
```jsx
import { Loading } from './components';

function DataTable({ data, loading }) {
  if (loading) {
    return <Loading type="skeleton" />;
  }

  return (
    <table>
      {/* Table content */}
    </table>
  );
}

function FullPageLoader() {
  return (
    <Loading
      type="spinner"
      size="lg"
      text="Loading application..."
      overlay
    />
  );
}
```

### `<ErrorBoundary>`

Error boundary component for catching and handling errors.

**Props:**
- `fallback` (ReactNode, optional): Custom error UI
- `onError` (function, optional): Error handler
- `resetOnPropsChange` (boolean, optional): Reset on prop changes
- `children` (ReactNode): Components to wrap

**Usage:**
```jsx
import { ErrorBoundary } from './components';

function App() {
  return (
    <ErrorBoundary
      fallback={
        <div className="error-page">
          <h1>Something went wrong</h1>
          <p>Please refresh the page and try again.</p>
        </div>
      }
      onError={(error, errorInfo) => {
        console.error('Error caught by boundary:', error);
        // Send to error reporting service
      }}
    >
      <MainApp />
    </ErrorBoundary>
  );
}
```

## Component Composition Examples

### Dashboard Layout

```jsx
import { 
  Container, 
  Grid, 
  Card, 
  Table, 
  Chart,
  Alert 
} from './components';

function Dashboard() {
  return (
    <Container size="xl">
      <Alert
        type="info"
        message="Welcome to your dashboard!"
        closable
      />
      
      <Grid columns={{ sm: 1, lg: 3 }} gap="1rem">
        <Card title="Total Users" shadow>
          <div className="stat-value">1,234</div>
          <div className="stat-change positive">+5.2%</div>
        </Card>
        
        <Card title="Revenue" shadow>
          <div className="stat-value">$12,345</div>
          <div className="stat-change positive">+12.1%</div>
        </Card>
        
        <Card title="Orders" shadow>
          <div className="stat-value">567</div>
          <div className="stat-change negative">-2.1%</div>
        </Card>
      </Grid>
      
      <Grid columns={{ sm: 1, lg: 2 }} gap="2rem">
        <Card title="Recent Orders" shadow>
          <Table
            data={recentOrders}
            columns={orderColumns}
            pagination={{ pageSize: 5 }}
          />
        </Card>
        
        <Card title="Sales Chart" shadow>
          <Chart
            type="line"
            data={salesData}
            height="300px"
          />
        </Card>
      </Grid>
    </Container>
  );
}
```

### Form Wizard

```jsx
import { 
  Form, 
  Input, 
  Select, 
  Button, 
  Container,
  Card 
} from './components';

function UserRegistration() {
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState({});

  const steps = [
    { title: 'Personal Info', component: PersonalInfoStep },
    { title: 'Account Details', component: AccountDetailsStep },
    { title: 'Confirmation', component: ConfirmationStep }
  ];

  return (
    <Container size="md">
      <Card title={`Step ${step}: ${steps[step - 1].title}`} shadow>
        <div className="step-indicator">
          {steps.map((_, index) => (
            <div 
              key={index}
              className={`step ${index + 1 <= step ? 'active' : ''}`}
            >
              {index + 1}
            </div>
          ))}
        </div>
        
        <Form
          initialValues={formData}
          onSubmit={(values) => {
            setFormData(values);
            if (step < steps.length) {
              setStep(step + 1);
            } else {
              // Submit form
            }
          }}
        >
          {React.createElement(steps[step - 1].component)}
          
          <div className="form-actions">
            {step > 1 && (
              <Button 
                variant="secondary" 
                onClick={() => setStep(step - 1)}
              >
                Previous
              </Button>
            )}
            <Button type="submit" variant="primary">
              {step === steps.length ? 'Submit' : 'Next'}
            </Button>
          </div>
        </Form>
      </Card>
    </Container>
  );
}
```

## Best Practices

### Component Documentation

```jsx
/**
 * Button component with multiple variants and states
 * 
 * @param {Object} props - Component props
 * @param {string} props.variant - Button style variant
 * @param {string} props.size - Button size
 * @param {boolean} props.disabled - Whether button is disabled
 * @param {boolean} props.loading - Show loading state
 * @param {Function} props.onClick - Click handler
 * @param {ReactNode} props.children - Button content
 * 
 * @example
 * <Button variant="primary" size="lg" onClick={handleClick}>
 *   Click me
 * </Button>
 */
export function Button({
  variant = 'primary',
  size = 'md',
  disabled = false,
  loading = false,
  onClick,
  children,
  ...props
}) {
  // Component implementation
}
```

### Accessibility Guidelines

- Always provide meaningful `alt` text for images
- Use proper ARIA labels and roles
- Ensure keyboard navigation works correctly
- Maintain proper color contrast ratios
- Include focus indicators for interactive elements
- Use semantic HTML elements when possible

### Performance Considerations

- Use `React.memo` for expensive components
- Implement lazy loading for large lists
- Optimize image sizes and formats
- Minimize re-renders with proper state management
- Use code splitting for large component libraries

---

*Last updated: 2024-01-01*