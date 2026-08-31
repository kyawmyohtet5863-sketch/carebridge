# CareBridge Flutter Mobile Application

The mobile application is parent-focused. It provides secure login, linked-child information, attendance summaries, published monthly results, teacher comments, and report history.

Recommended initialization:

```bash
flutter create --org com.carebridge --platforms=android,ios .
```

Required capabilities:

- Typed Laravel API client
- Platform secure storage for tokens
- Route guards and logout
- Loading, empty, offline, and error states
- Unit, widget, and integration tests

Follow the feature structure in [PROJECT_STRUCTURE.md](../PROJECT_STRUCTURE.md).
