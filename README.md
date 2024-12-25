# Secure Database Application with AES Encryption

## Technical Overview

This project implements a secure database application with AES encryption for sensitive data protection. The system utilizes a modular architecture with dedicated components for encryption, database operations, and user interface management.

### Core Components

#### 1. Encryption System
The encryption system is built around the Advanced Encryption Standard (AES) with the following specifications:

- **Algorithm**: AES in CBC (Cipher Block Chaining) mode
- **Key Size**: 192-bit (24 bytes)
- **Padding**: PKCS5Padding
- **Provider**: BouncyCastle Security Provider
- **Encoding**: Base64 for encrypted output

Key components:
- `EncryptionHelper`: Core encryption/decryption operations
- `EncryptionUtils`: Utility functions for IV generation and byte conversions

```java
// Example encryption flow:
plaintext → UTF-8 bytes → AES/CBC encryption → Base64 encoding → ciphertext
```

#### 2. Database Architecture

The database system uses H2 with a layered architecture:

```
DatabaseHelper (Core)
    ├── UserHelper
    ├── GroupArticlesHelper
    └── MessageHelper
```

Each helper manages specific database operations:
- `UserHelper`: User authentication and profile management
- `GroupArticlesHelper`: Group and article operations with encryption
- `MessageHelper`: Message handling and search history

#### 3. Security Implementation

##### Data Protection
The system encrypts multiple types of sensitive data:
- User credentials (passwords, emails)
- Group content
- Article data

##### Encryption Process
1. **Initialization Vector (IV) Generation**:
   - 16-byte IV derived from input string
   - Used in CBC mode for added security

2. **Key Management**:
   ```java
   SecretKey key = new SecretKeySpec(keyBytes, "AES");
   cipher.init(Cipher.ENCRYPT_MODE, key, new IvParameterSpec(IV));
   ```

3. **Data Flow**:
   ```
   Encryption: String → Bytes → AES/CBC → Base64
   Decryption: Base64 → AES/CBC → Bytes → String
   ```

### Technical Considerations

#### 1. Cryptographic Implementation
- Uses BouncyCastle provider for enhanced security features
- CBC mode provides better security than ECB by chaining blocks
- PKCS5Padding ensures proper block alignment

#### 2. Database Security
- Encrypted storage of sensitive fields
- Prepared statements to prevent SQL injection
- Transaction management for data integrity

#### 3. Input Validation
Regular expressions enforce:
- Password complexity requirements
- Email format validation
- Username constraints

```java
public static String password_regex = 
    "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)(?=.*[@#$%^&+=])[A-Za-z\\d@#$%^&+=]{8,}$";
```

### Production Considerations

Current implementation notes for production deployment:

1. **Key Management**:
   - Current: Static key and IV
   - Recommendation: Implement proper key rotation and management
   - Consider using a key management service (KMS)

2. **Encryption Improvements**:
   - Generate unique IVs per encryption operation
   - Implement salt for password hashing
   - Add key rotation mechanism

3. **Security Enhancements**:
   - Add rate limiting for authentication attempts
   - Implement session management
   - Add audit logging for sensitive operations

### Architecture Diagram
```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Controllers   │     │  Helper Classes  │     │    Database     │
│  (JavaFX/FXML)  │────▶│ (With Encryption│────▶│    (H2 with     │
│                 │     │    Layer)        │     │ Encrypted Data) │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## Development Setup

### Prerequisites
- Java Development Kit (JDK)
- JavaFX
- H2 Database
- BouncyCastle Security Provider

### Building and Running
1. Clone the repository
2. Set up the database connection in `DatabaseHelper.java`
3. Run the main application class

### Security Notes
- Do not commit encryption keys
- Use environment variables for sensitive configuration
- Follow secure coding practices when extending functionality

## Contributing
When contributing to this project:
1. Follow the existing encryption patterns
2. Add comprehensive unit tests
3. Document security implications of changes
4. Review the cryptographic impact of modifications

## License
[Add appropriate license information]
