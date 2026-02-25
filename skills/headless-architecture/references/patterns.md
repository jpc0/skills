# Headless Core Architectural Patterns

Concrete implementations of common Headless Core patterns.

## 1. The Validator (Forms & Input)
Business-critical validation resides in the Core, while the UI only handles the draft state.

### Rust Core Example
```rust
// core/src/domain/validator.rs
pub struct Validator {
    pub min_length: usize,
    pub require_digit: bool,
}

impl Validator {
    pub fn validate_password(&self, password: &str) -> Vec<String> {
        let mut errors = vec![];
        if password.len() < self.min_length {
            errors.push(format!("Password must be at least {} characters", self.min_length));
        }
        if self.require_digit && !password.chars().any(|c| c.is_digit(10)) {
            errors.push("Password must contain at least one digit".into());
        }
        errors
    }
}
```

### TypeScript React View Example
```tsx
// ui/src/components/PasswordForm.tsx
const PasswordForm = () => {
  const [draft, setDraft] = useState("");
  const [errors, setErrors] = useState<string[]>([]);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const val = e.target.value;
    setDraft(val);
    // Call Core bridge
    const coreErrors = Core.validatePassword(val);
    setErrors(coreErrors);
  };

  return (
    <div>
      <input type="password" value={draft} onChange={handleChange} />
      {errors.map(err => <span key={err}>{err}</span>)}
    </div>
  );
};
```

## 2. The Codec (Real-time Sync)
The Core acts as a buffer for noisy packets, applying deltas and handling sync/interpolation.

### Rust Core Example
```rust
// core/src/codec/sync.rs
pub struct StockCodec {
    buffer: VecDeque<f64>,
    capacity: usize,
}

impl StockCodec {
    pub fn push_raw_price(&mut self, price: f64) {
        if self.buffer.len() >= self.capacity {
            self.buffer.pop_front();
        }
        self.buffer.push_back(price);
    }

    pub fn get_smoothed_price(&self) -> f64 {
        if self.buffer.is_empty() { return 0.0; }
        self.buffer.iter().sum::<f64>() / self.buffer.len() as f64
    }
}
```

## 3. The Pointer (Heavy Assets)
The View only browses lightweight Metadata/IDs. The Core resolves IDs to actual resources.

### Rust Core Example
```rust
// core/src/data/pointer.rs
pub struct VideoLibrary {
    storage: HashMap<VideoId, PathBuf>,
}

impl VideoLibrary {
    pub fn resolve_stream(&self, id: &VideoId) -> Option<VideoStream> {
        let path = self.storage.get(id)?;
        Some(VideoStream::open(path))
    }
}
```
