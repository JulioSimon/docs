# Generic WordPress Functionalities

This document provides a comprehensive overview of the generic WordPress functionalities available in the Tonga National Portal administration panel.

## Authentication System

### Definition

The WordPress Authentication System is a comprehensive security framework that manages user identity verification and access control within the WordPress administration panel. It ensures that only authorized users can access the admin dashboard and its various functionalities based on their assigned roles and permissions.

Key components of the authentication system include:

- **User credentials storage**: Securely stores usernames and password hashes in the WordPress database
- **Login mechanism**: Processes user login attempts and validates credentials
- **Session management**: Creates and maintains secure user sessions after successful authentication
- **Access control**: Restricts access to specific areas based on user roles and capabilities
- **Security features**: Implements protection against unauthorized access attempts

The authentication system serves as the gateway to the WordPress administration panel, providing a balance between security and usability while protecting sensitive site data and functionality.

### Login Process

The WordPress login process follows a structured sequence of steps to verify user identity and establish a secure session. Below is a detailed explanation of the standard login workflow:

1. **Accessing the Login Page**
   - Navigate to the login URL: `/wp-login.php` (typically https://yourdomain.com/wp-login.php)
   - The system presents a login form requesting username/email and password
   - [Screenshot Recommended] *Login form showing username and password fields*

2. **Credential Submission**
   - User enters their username or email address and password
   - User clicks the "Log In" button or presses Enter to submit credentials
   - The form data is submitted via POST request to the server

3. **Server-side Validation**
   - WordPress validates the submitted credentials against stored user data
   - The system checks if the username/email exists in the database
   - If found, the submitted password is verified against the stored password hash
   - Failed login attempts may trigger security measures (like temporary lockouts)

4. **Session Establishment**
   - Upon successful validation, WordPress creates a new authenticated session
   - Authentication cookies are set in the user's browser:
     - `wordpress_[hash]`: Contains the authenticated user information
     - `wordpress_logged_in_[hash]`: Indicates the user is logged in
     - `wordpress_sec_[hash]`: Used for password-based cookie authentication
   - These cookies are encrypted and contain expiration timestamps

5. **Redirection**
   - New users are typically directed to their profile page
   - Returning users are redirected to:
     - The admin dashboard by default
     - A specific page if a redirect parameter was included in the login URL
     - The page they were attempting to access before being prompted to log in
   - [Screenshot Recommended] *Admin dashboard after successful login*

**Important Security Considerations:**

- WordPress uses nonces (number used once) to prevent CSRF attacks during login
- Failed login attempts are logged and may trigger security measures
- The login page can be protected with additional security layers (like 2FA)
- All login communication should occur over HTTPS to prevent credential interception

### Authentication Methods

WordPress supports multiple authentication methods to accommodate different security requirements and user preferences. Each method offers distinct advantages in terms of security, convenience, and implementation complexity.

#### 1. Standard Username/Password Authentication

The default authentication method built into WordPress core:

- **Implementation**: Users enter their username/email and password on the login form
- **Security Level**: Basic (can be enhanced with password policies)
- **User Experience**: Familiar login process requiring memorization of credentials
- **Best Practices**:
  - Enforce strong password policies
  - Implement account lockouts after failed attempts
  - Regularly prompt for password changes

#### 2. Two-Factor Authentication (2FA)

Adds an additional verification layer beyond passwords:

- **Implementation**: Requires plugins like "Two Factor Authentication" or "Wordfence"
- **Security Level**: High (significantly reduces account compromise risk)
- **Methods**:
  - Time-based One-Time Passwords (TOTP) via authenticator apps
  - SMS verification codes
  - Email verification codes
  - Hardware security keys (like YubiKey)
- **User Flow**:
  1. User enters username/password
  2. System prompts for the second factor
  3. User provides the verification code or uses the security key
  - [Screenshot Recommended] *2FA verification screen showing code entry field*

#### 3. Single Sign-On (SSO)

Allows authentication through external identity providers:

- **Implementation**: Requires plugins like "OAuth Single Sign On" or "SAML SSO"
- **Security Level**: Varies based on the identity provider's security
- **Providers**:
  - Google Workspace
  - Microsoft Azure AD
  - Facebook
  - Twitter
  - Other OAuth/SAML providers
- **Benefits**:
  - Reduces password fatigue
  - Centralizes authentication management
  - Often includes additional security features

#### 4. Application Passwords

Secure method for API and application access:

- **Implementation**: Built into WordPress core (since version 5.6)
- **Security Level**: High (limited-scope access tokens)
- **Use Cases**:
  - Mobile apps
  - REST API access
  - Programmatic site management
- **Management**: Users can create and revoke application-specific passwords from their profile

#### 5. Cookie-Based Authentication

The standard method WordPress uses to maintain sessions:

- **Implementation**: Built into WordPress core
- **Security Level**: Moderate
- **Cookie Types**:
  - Authentication cookies
  - Login cookies
  - Session cookies
- **Security Considerations**:
  - Cookies are encrypted
  - Include expiration timestamps
  - Can be configured for different lifespans

**Implementation Considerations:**

- Multiple authentication methods can be used simultaneously
- Authentication can be customized via hooks and filters
- Enterprise environments often require more robust methods (SSO, 2FA)
- All authentication should occur over HTTPS connections

### Logout Process

The WordPress logout process securely terminates user sessions and removes authentication cookies to prevent unauthorized access. Understanding this process is crucial for maintaining security and managing user sessions effectively.

#### Standard Logout Procedure

1. **Initiating Logout**
   - User clicks the "Log Out" link in the admin bar or profile menu
   - This link points to: `wp-login.php?action=logout`
   - A nonce parameter is included to prevent CSRF attacks
   - [Screenshot Recommended] *Admin bar highlighting the logout option*

2. **Server-side Processing**
   - WordPress validates the logout request and nonce
   - The system clears the user's session data from the server
   - All WordPress authentication cookies are invalidated:
     - `wordpress_[hash]`
     - `wordpress_logged_in_[hash]`
     - `wordpress_sec_[hash]`

3. **Confirmation and Redirection**
   - User receives confirmation of successful logout
   - System redirects to:
     - The login page by default
     - A custom URL if specified in the configuration
     - [Screenshot Recommended] *Logout confirmation screen*

#### Automatic Logout Scenarios

WordPress may automatically terminate sessions under certain conditions:

- **Session Expiration**
  - Authentication cookies have built-in expiration times
  - Default session length is 2 days (configurable)
  - After expiration, users must re-authenticate

- **Password Changes**
  - When a user changes their password, all existing sessions are terminated
  - This security measure prevents access with compromised credentials

- **User Role Changes**
  - Modifications to a user's role or capabilities may trigger session termination
  - Ensures users can't retain access beyond their current permissions

- **Manual Session Management**
  - Administrators can force-logout users using plugins
  - Useful for security incidents or maintenance periods

#### Security Best Practices

- **Complete Logout**
  - Users should be instructed to close their browser after logout for complete session termination
  - Some browser features (like session restore) may retain cookies

- **Idle Session Timeout**
  - Consider implementing automatic logout after periods of inactivity
  - Requires additional plugins or custom code
  - Particularly important for shared computers or public environments

- **Logout Verification**
  - Confirm successful logout to users
  - Provide clear instructions if additional steps are needed

- **Multi-device Management**
  - Modern WordPress installations track login sessions across devices
  - Users can view and terminate specific sessions from their profile

**Technical Implementation Note:**
The logout process uses the `wp_logout()` function which triggers the 'wp_logout' action hook, allowing developers to perform additional cleanup or logging operations during logout.

## Users Management

### Definition

### User Addition Process

### Sending Password Reset

### User Roles

1. **Super Administrator**

2. **Administrator**

3. **Editor**

4. **Author**

5. **Contributor**

6. **Subscriber**


## Navigation Menu Management

### Definition

#### Adding a New Item Menu

#### Manage Special Submenus Using Bricks Plugin

#### Manage Menu Translations Using WPML Plugin


## Content Management

### Posts

#### Definition

#### New Post Creation Process

##### Creation of the Post using Guttemberg Editor Process

##### Creation of the Post using Bricks Editor Process

#### Post Metadata Management

#### Post Featured Image

#### Post Taxonomies

#### Post Translation Management with WPML Plugin

#### Post Social Sharing with FS Poster Plugin

### Pages

#### Definition

#### New Page Creation Process

##### Creation of the Page using Guttemberg Editor Process

##### Creation of the Page using Bricks Editor Process

#### Page Metadata Management

#### Page Translation Management with WPML Plugin

#### Page Social Sharing with FS Poster Plugin


## Media Library

### Definition

### View All Media

### Add New Media


## Platform Settings

### Basic Site Configuration

### Media Settings
