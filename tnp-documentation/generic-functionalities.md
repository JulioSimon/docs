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

The WordPress Users Management system is a comprehensive framework that allows administrators to create, modify, and manage user accounts within the WordPress platform. It provides tools for controlling access to the website's administrative functions based on predefined roles and capabilities.

Key components of the Users Management system include:

- **User accounts**: Individual profiles containing personal information, credentials, and role assignments
- **Role-based access control**: Predefined sets of permissions that determine what actions users can perform
- **User capabilities**: Granular permissions that can be assigned or removed from specific roles
- **User metadata**: Additional information associated with user accounts
- **Authentication management**: Tools for password resets, account recovery, and security

The Users Management system serves as the foundation for WordPress's security model, ensuring that users have appropriate access to site functionality while protecting sensitive operations from unauthorized access.

### User Addition Process

Adding new users to a WordPress site follows a structured workflow that can be performed by administrators. Below is a detailed explanation of the user addition process:

1. **Accessing the Users Section**
   - Log in to the WordPress admin dashboard
   - Navigate to "Users" > "Add New" in the left sidebar menu
   - [Screenshot Recommended] *Users menu in the WordPress admin sidebar*

2. **Completing the New User Form**
   - Fill in the required fields:
     - Username (required): A unique identifier that cannot be changed later
     - Email (required): A valid email address for the user
     - Password (optional): A strong password or leave blank to auto-generate
     - Confirm Password (if entering manually)
     - First Name (optional)
     - Last Name (optional)
     - Website (optional)
   - [Screenshot Recommended] *Add New User form with all fields visible*

3. **Setting the User Role**
   - Select an appropriate role from the dropdown menu:
     - Administrator
     - Editor
     - Author
     - Contributor
     - Subscriber
   - The selected role determines what actions the user can perform on the site
   - [Screenshot Recommended] *Role selection dropdown showing available options*

4. **Additional Options**
   - "Send User Notification" checkbox: When selected, WordPress sends the new user an email with their account details
   - In multisite installations, additional options may appear for site assignment

5. **Creating the User Account**
   - Click the "Add New User" button to create the account
   - WordPress validates the information and creates the user account
   - A success message appears confirming the user creation
   - [Screenshot Recommended] *Success message after user creation*

**Important Considerations:**

- Usernames must be unique across the entire WordPress installation
- Email addresses must be unique and valid, as they are used for account recovery
- Passwords should follow security best practices (minimum length, complexity)
- Consider the principle of least privilege when assigning roles (assign only the permissions necessary for the user's tasks)
- In multisite installations, users can be added to specific sites or the entire network

### Sending Password Reset

The WordPress password reset functionality allows administrators to help users regain access to their accounts when they forget their passwords. This process maintains security while providing a straightforward recovery mechanism.

#### Administrator-Initiated Password Reset

1. **Accessing User Management**
   - Log in to the WordPress admin dashboard
   - Navigate to "Users" > "All Users" in the left sidebar
   - [Screenshot Recommended] *Users listing page showing all user accounts*

2. **Locating the User**
   - Find the user account that needs a password reset using the search box or by browsing the list
   - Hover over the user's name to reveal action links

3. **Initiating the Reset**
   - Click on the "Send password reset" link that appears when hovering over the user's name
   - [Screenshot Recommended] *Hover state showing the "Send password reset" link*

4. **Confirmation**
   - WordPress displays a confirmation message indicating that the password reset email has been sent
   - The system automatically generates a unique, secure reset link and sends it to the user's registered email address
   - [Screenshot Recommended] *Confirmation message after sending reset email*

#### User-Initiated Password Reset

1. **Accessing the Login Page**
   - User navigates to the WordPress login page (typically /wp-login.php)
   - Clicks on the "Lost your password?" link below the login form
   - [Screenshot Recommended] *Login form highlighting the "Lost your password?" link*

2. **Requesting Password Reset**
   - User enters their username or email address in the provided field
   - Clicks the "Get New Password" button
   - [Screenshot Recommended] *Password reset request form*

3. **Email Delivery**
   - WordPress sends a password reset link to the user's registered email address
   - The email contains a secure, time-limited link to reset the password

4. **Setting a New Password**
   - User clicks the link in the email, which directs them to a password reset page
   - User enters a new password (and confirms it)
   - Clicks "Reset Password" to save the changes
   - [Screenshot Recommended] *New password creation form*

5. **Confirmation and Login**
   - WordPress confirms the password has been changed
   - User can now log in with the new password

**Security Considerations:**

- Password reset links expire after a limited time (typically 24 hours)
- Reset links can only be used once
- The system validates that the request comes from the same IP address that initiated it
- Failed reset attempts are logged for security monitoring
- Consider implementing additional security measures like two-factor authentication for sensitive accounts

### User Roles

WordPress implements a role-based access control system that assigns specific capabilities to predefined user roles. Each role represents a set of permissions that determine what actions users can perform within the WordPress administration panel.

1. **Super Administrator**
   
   The Super Administrator role exists only in WordPress Multisite installations and has complete control over the entire network of sites.
   
   **Capabilities include:**
   - All Administrator capabilities for every site in the network
   - Manage network settings and configurations
   - Create and delete sites within the network
   - Install and activate themes and plugins network-wide
   - Manage user accounts across all sites
   - Add and remove sites from the network
   - Upgrade WordPress core, themes, and plugins for the entire network
   
   **Best suited for:**
   - Technical managers responsible for the entire WordPress network
   - Organization owners or highest-level administrators
   
   **Security note:** This role should be assigned very sparingly due to its extensive capabilities.

2. **Administrator**
   
   The Administrator role has complete control over a single WordPress site, with access to all administrative features.
   
   **Capabilities include:**
   - Manage all site content (posts, pages, media)
   - Install, activate, and delete themes and plugins
   - Add and manage users
   - Modify site settings and options
   - Edit theme files and customize site appearance
   - Import and export site content
   - Update WordPress core, themes, and plugins
   - Moderate comments
   - [Screenshot Recommended] *Administrator capabilities in the user profile screen*
   
   **Best suited for:**
   - Site owners
   - Primary site managers
   - Technical administrators
   
   **Security note:** Limit the number of administrators to reduce security risks.

3. **Editor**
   
   The Editor role has full control over content management but cannot modify site settings or access technical features.
   
   **Capabilities include:**
   - Create, edit, publish, and delete any posts or pages (including those by other users)
   - Moderate comments
   - Manage categories, tags, and links
   - Upload files and media
   - Access to the WordPress dashboard
   
   **Best suited for:**
   - Content managers
   - Editorial team leaders
   - Senior content creators
   
   **Security note:** Editors cannot install plugins or themes, which helps maintain site security.

4. **Author**
   
   The Author role can create and manage their own content but has limited access to content created by others.
   
   **Capabilities include:**
   - Create, edit, publish, and delete their own posts
   - Upload files and media for their own content
   - View comments, including those awaiting moderation
   - Access to the WordPress dashboard
   
   **Best suited for:**
   - Regular content contributors
   - Blog writers
   - Staff members who need to publish content
   
   **Security note:** Authors cannot edit or delete content created by other users.

5. **Contributor**
   
   The Contributor role can create and edit their own posts but cannot publish them directly.
   
   **Capabilities include:**
   - Create and edit their own posts (but not publish them)
   - Cannot upload media files
   - View comments on their own posts
   - Access to the WordPress dashboard
   
   **Best suited for:**
   - Guest writers
   - Infrequent content contributors
   - New team members during probation periods
   
   **Security note:** All content from Contributors requires review and approval by an Editor or Administrator before publication.

6. **Subscriber**
   
   The Subscriber role has minimal capabilities, primarily focused on managing their own profile.
   
   **Capabilities include:**
   - Read content (including private content if enabled)
   - Manage their own profile
   - Change their password
   - Comment on posts (if comments are enabled)
   
   **Best suited for:**
   - Registered readers
   - Newsletter subscribers
   - Community members
   - Customers who need access to protected content
   
   **Security note:** Subscribers have no publishing capabilities and cannot make changes to site content or settings.

**Role Management Best Practices:**

- Regularly audit user roles to ensure appropriate access levels
- Follow the principle of least privilege when assigning roles
- Consider using role management plugins for more granular control
- Document which roles have access to sensitive operations
- Create custom roles for specific workflow needs when standard roles are insufficient

## Navigation Menu Management

### Definition

The WordPress Navigation Menu Management system is a comprehensive framework that allows administrators to create, organize, and customize website navigation menus. It provides a user-friendly interface for controlling the structure, appearance, and functionality of navigation elements throughout the website.

Key components of the Navigation Menu Management system include:

- **Menu locations**: Theme-defined positions where menus can be displayed (header, footer, sidebar)
- **Menu items**: Individual navigation elements that can include pages, posts, custom links, and other content types
- **Menu hierarchy**: Parent-child relationships that create dropdown or nested menu structures
- **Custom link support**: Ability to add links to external websites or specific sections within the site
- **Menu customization**: Options for adding CSS classes, setting link targets, and customizing item appearance

The Navigation Menu Management system serves as the primary tool for creating intuitive site navigation, ensuring visitors can easily find and access content throughout the WordPress website.

### Adding a New Item Menu

Adding new items to a WordPress navigation menu follows a structured workflow that can be performed by administrators or users with menu editing capabilities. Below is a detailed explanation of the menu item addition process:

1. **Accessing the Menu Editor**
   - Log in to the WordPress admin dashboard
   - Navigate to "Appearance" > "Menus" in the left sidebar
   - [Screenshot Recommended] *Appearance menu in the WordPress admin sidebar highlighting the Menus option*

2. **Selecting a Menu to Edit**
   - Choose an existing menu from the dropdown at the top of the page, or
   - Create a new menu by clicking the "create a new menu" link
   - Enter a name for the new menu if creating one
   - Click "Create Menu" button
   - [Screenshot Recommended] *Menu selection dropdown and creation form*

3. **Adding Items to the Menu**
   - Use the panels on the left side of the screen to select items to add:
     - **Pages**: Select from published pages
     - **Posts**: Select from published posts
     - **Custom Links**: Enter a URL and link text
     - **Categories**: Select from content categories
     - **Tags**: Select from content tags
     - **Post Types**: Select from custom post types (if available)
   - Check the boxes next to items you want to add
   - Click the "Add to Menu" button
   - [Screenshot Recommended] *Left panel showing available items with checkboxes and Add to Menu button*

4. **Organizing Menu Structure**
   - Drag and drop menu items to reorder them
   - Drag items slightly to the right to create child items (creating dropdown menus)
   - Click on the arrow icon on a menu item to reveal additional options:
     - **Navigation Label**: The text displayed in the menu
     - **Title Attribute**: Text that appears when hovering over the menu item
     - **Open link in a new tab**: Checkbox to control link target behavior
     - **CSS Classes**: Field to add custom CSS classes
     - **Link Relationship (XFN)**: Define the relationship between the linked page and your site
     - **Description**: Additional text that some themes display with menu items
   - [Screenshot Recommended] *Menu item being dragged to create a hierarchical structure*

5. **Assigning Menu Location**
   - In the "Menu Settings" section at the bottom of the page, check the appropriate display location(s)
   - Available locations depend on the active theme (common locations include Primary Menu, Footer Menu, etc.)
   - [Screenshot Recommended] *Menu Settings section showing theme locations*

6. **Saving the Menu**
   - Click the "Save Menu" button to apply all changes
   - A success message will appear confirming the menu has been updated
   - [Screenshot Recommended] *Success message after saving menu changes*

**Important Considerations:**

- Menu items can be rearranged at any time by dragging and dropping
- Items can be removed by clicking the "Remove" link in the expanded item options
- Most themes support multiple menu locations, allowing different menus for different areas of the site
- The "Screen Options" tab at the top of the page allows enabling additional fields for menu items
- Custom menu items can be created using the "Links" box to point to any URL, including external websites

### Manage Special Submenus Using Bricks Plugin

The Bricks Builder plugin provides advanced capabilities for creating and managing specialized navigation submenus with enhanced styling and functionality. Below is a detailed guide on managing special submenus using the Bricks plugin:

1. **Accessing Bricks Builder Interface**
   - Log in to the WordPress admin dashboard
   - Navigate to "Bricks" in the left sidebar
   - Select the template or page where you want to manage navigation
   - Click "Edit with Bricks" to open the builder interface
   - [Screenshot Recommended] *Bricks menu in WordPress admin and Edit with Bricks button*

2. **Adding a Navigation Menu Element**
   - In the Bricks builder, click the "+" icon to add a new element
   - Search for "Nav Menu" in the elements panel
   - Drag the Nav Menu element to your desired location in the layout
   - [Screenshot Recommended] *Bricks elements panel with Nav Menu element highlighted*

3. **Configuring the Basic Menu Settings**
   - In the element settings panel (right side):
     - Select your WordPress menu from the "Menu" dropdown
     - Choose the menu layout type (horizontal or vertical)
     - Set the alignment (left, center, right)
     - Configure spacing between menu items
   - [Screenshot Recommended] *Basic menu settings panel in Bricks*

4. **Creating Special Submenu Styles**
   - Navigate to the "Dropdown" tab in the settings panel
   - Configure dropdown settings:
     - **Trigger**: Hover, click, or hover intent
     - **Animation**: Select entrance animation for submenus
     - **Width**: Set fixed or auto width for dropdown menus
     - **Position**: Adjust the position relative to parent items
     - **Offset**: Fine-tune the submenu positioning
   - [Screenshot Recommended] *Dropdown settings panel showing animation and positioning options*

5. **Styling Submenu Appearance**
   - In the "Style" tab:
     - Configure background colors, borders, and shadows for submenus
     - Set typography for submenu items
     - Add custom padding and margins
     - Configure hover effects and transitions
   - Use the "Submenu Item" section to style individual items within dropdowns
   - [Screenshot Recommended] *Style tab showing submenu appearance settings*

6. **Creating Mega Menus**
   - Enable the "Mega Menu" option for specific top-level menu items
   - Set the number of columns for the mega menu
   - Configure column widths and content alignment
   - Add custom elements within mega menu panels:
     - Images
     - Text blocks
     - Buttons
     - Dividers
     - Custom HTML
   - [Screenshot Recommended] *Mega menu configuration panel with column settings*

7. **Adding Custom Interactions**
   - Navigate to the "Interactions" tab
   - Add custom effects for different user actions:
     - Mouse enter/leave
     - Click/tap
     - Scroll
   - Configure animations, transitions, and transforms
   - Set timing and easing functions
   - [Screenshot Recommended] *Interactions panel with animation settings*

8. **Mobile Menu Configuration**
   - Switch to the "Mobile" tab
   - Set the breakpoint where the menu converts to mobile view
   - Choose the mobile menu type (off-canvas, dropdown, fullscreen)
   - Configure the mobile menu toggle button appearance
   - Set animation and transition effects for mobile menu
   - [Screenshot Recommended] *Mobile menu settings showing toggle button configuration*

**Important Points:**

- Bricks allows for completely different styling between main menu items and submenu items
- Custom CSS classes can be added to specific menu items for targeted styling
- The responsive preview mode helps test how menus appear on different device sizes
- Changes made in Bricks do not affect the original WordPress menu structure, only its presentation
- For complex layouts, consider using the Bricks Container element to wrap menu items
- Always test menu interactions thoroughly, especially for touch devices

### Manage Menu Translations Using WPML Plugin

The WPML (WordPress Multilingual) plugin provides comprehensive tools for translating navigation menus across multiple languages. Below is a detailed guide on managing menu translations using the WPML plugin:

1. **Verifying WPML Configuration**
   - Log in to the WordPress admin dashboard
   - Navigate to "WPML" > "Languages" in the left sidebar
   - Ensure all required languages are added and active
   - Check that language switcher settings are configured
   - [Screenshot Recommended] *WPML Languages settings page showing active languages*

2. **Accessing the Menu Translation Interface**
   - Navigate to "Appearance" > "Menus" in the left sidebar
   - Select the menu you want to translate from the dropdown
   - Look for the WPML language tabs at the top of the menu editor
   - [Screenshot Recommended] *Menu editor showing WPML language tabs*

3. **Creating a Translated Menu**
   - Click on the tab for the language you want to create a translation for
   - You'll see two options:
     - **Create new**: Create a completely new menu for this language
     - **Translate existing**: Create a translated version of the current menu
   - Select "Translate existing" to maintain the same structure
   - Click "Create" to generate the translated menu
   - [Screenshot Recommended] *WPML menu creation options dialog*

4. **Translating Menu Items**
   - After creating the translated menu, you'll see the original menu structure
   - Each menu item will have a "Translate" link next to it
   - Click "Translate" to open the translation panel for that item
   - Enter the translated text for:
     - **Navigation Label**: The visible menu text
     - **Title Attribute**: The tooltip text (if used)
   - Click "Save" to apply the translation
   - [Screenshot Recommended] *Menu item translation panel with text fields*

5. **Synchronizing Menu Structure**
   - If you make structural changes to your primary language menu:
     - Navigate to "WPML" > "Navigation Menus"
     - Select the menu you want to synchronize
     - Check the languages you want to update
     - Click "Synchronize" to apply the structural changes across languages
   - [Screenshot Recommended] *WPML Navigation Menus synchronization screen*

6. **Managing Language-Specific Menu Items**
   - Some menu items may need to be different in specific languages
   - To create language-specific items:
     - Switch to the specific language tab in the menu editor
     - Add new items that should appear only in that language
     - These items won't appear in other language versions
   - [Screenshot Recommended] *Adding language-specific menu items*

7. **Assigning Menus to Language-Specific Locations**
   - Navigate to the "Manage Locations" tab in the menu editor
   - You'll see location options for each active language
   - Assign the appropriate translated menu to each language's locations
   - Click "Save Changes" to apply the assignments
   - [Screenshot Recommended] *Menu locations panel showing language-specific assignments*

8. **Testing Menu Translations**
   - Use the language switcher on your live site to verify:
     - Menu items display correctly in each language
     - Links point to the correct translated content
     - Dropdown functionality works properly
     - Mobile menu displays translations correctly
   - [Screenshot Recommended] *Front-end view showing translated menu in different languages*

**Important Considerations:**

- Menu items linked to translated content (like pages or posts) will automatically link to the correct language version
- Custom links need manual translation, including the URL if it points to language-specific content
- Menu structure synchronization only affects the hierarchy, not the content of menu items
- Some themes may require additional configuration to properly display translated menus
- WPML's String Translation module can be used for translating menu-related theme strings
- Regular testing across languages is essential after making menu changes
- Consider using WPML's Advanced Translation Editor for more complex menu translation projects


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
