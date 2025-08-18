# React Components `v 18.x`

## Essentials

```jsx
// Functional component with arrow function
const Button = ({ text, onClick, disabled = false }) => (
  <button onClick={onClick} disabled={disabled}>
    {text}
  </button>
);

// Component with children
const Card = ({ children, title }) => (
  <div className="card">
    <h2>{title}</h2>
    {children}
  </div>
);
```

**Key Concepts**: Props | Children | Composition | Reusability
**When to use**: Building reusable UI pieces that accept data and render consistently

## Syntax Reference

### Basic Structure

```jsx
// Simple functional component
const Welcome = () => <h1>Hello World</h1>;

// Component with props
const Greeting = ({ name, age }) => (
  <div>
    <h1>Hello, {name}!</h1>
    <p>You are {age} years old</p>
  </div>
);

// Component with destructured props and default values
const UserCard = ({ 
  name, 
  email, 
  avatar = '/default-avatar.png',
  isActive = false 
}) => (
  <div className={`user-card ${isActive ? 'active' : ''}`}>
    <img src={avatar} alt={`${name}'s avatar`} />
    <h3>{name}</h3>
    <p>{email}</p>
  </div>
);
```

### Common Patterns

```jsx
// Pattern 1: Children prop for composition
const Modal = ({ children, isOpen, onClose }) => {
  if (!isOpen) return null;
  
  return (
    <div className="modal-backdrop" onClick={onClose}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        {children}
      </div>
    </div>
  );
};

// Usage: <Modal><p>Modal content here</p></Modal>

// Pattern 2: Default props with destructuring
const BlogPost = ({ 
  title, 
  content, 
  author = 'Anonymous',
  publishDate = new Date().toLocaleDateString(),
  tags = []
}) => (
  <article>
    <h1>{title}</h1>
    <div className="meta">
      By {author} on {publishDate}
    </div>
    <div className="content">{content}</div>
    <div className="tags">
      {tags.map(tag => <span key={tag} className="tag">{tag}</span>)}
    </div>
  </article>
);

// Pattern 3: Prop types validation (requires import)
import PropTypes from 'prop-types';

const Product = ({ name, price, inStock }) => (
  <div className={`product ${!inStock ? 'out-of-stock' : ''}`}>
    <h3>{name}</h3>
    <p>${price}</p>
    {!inStock && <span className="status">Out of Stock</span>}
  </div>
);

Product.propTypes = {
  name: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
  inStock: PropTypes.bool
};

Product.defaultProps = {
  inStock: true
};

// Pattern 4: Container vs Presentational separation
// Container Component (handles logic/state)
const UserListContainer = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetchUsers().then(data => {
      setUsers(data);
      setLoading(false);
    });
  }, []);
  
  return <UserList users={users} loading={loading} />;
};

// Presentational Component (handles display only)
const UserList = ({ users, loading }) => {
  if (loading) return <div>Loading...</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>
          <UserCard {...user} />
        </li>
      ))}
    </ul>
  );
};
```

## Do's & Don'ts

| ✅ Do | ❌ Don't |
|-------|----------|
| Use arrow functions for simple components | Use function declarations unnecessarily |
| Destructure props in parameters | Access props.propName repeatedly |
| Provide default values for optional props | Assume props exist without defaults |
| Use PropTypes for type checking | Skip prop validation in reusable components |
| Keep components focused on single responsibility | Mix data fetching with presentation |
| Use composition with children prop | Deeply nest component hierarchies |
| Name components with PascalCase | Use camelCase or snake_case for components |

## Quick Fixes

- **"Cannot read property of undefined"** → Add default props or optional chaining
- **Component not re-rendering** → Check if props are actually changing (use React DevTools)
- **PropTypes warnings in console** → Match prop types with actual data being passed
- **Key prop missing warnings** → Add unique `key` prop when rendering lists
- **Component not receiving children** → Ensure `children` prop is destructured and used

## Gotchas

⚠️ **Props are read-only**: Never mutate props directly - treat them as immutable data
⚠️ **Children prop gotcha**: `children` can be undefined, a single element, or an array - handle accordingly
⚠️ **Default props timing**: Default props are applied before PropTypes validation runs
⚠️ **Arrow function binding**: Arrow functions in JSX create new functions on each render - use useCallback for performance
⚠️ **Prop drilling**: Passing props through many component levels - consider Context API for deep hierarchies

## Checklist

- [ ] Component uses arrow function syntax consistently
- [ ] Props are destructured in function parameters with defaults where needed
- [ ] PropTypes are defined for reusable components
- [ ] Component has single, clear responsibility
- [ ] Children prop is used when component wraps other content
- [ ] Container and presentational concerns are separated when appropriate
- [ ] Component is named with PascalCase and exported properly