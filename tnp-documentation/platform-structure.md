# Platform Structure

This document provides a comprehensive overview of the WordPress architecture and structure of the Tonga National Portal platform.

## WordPress Architecture Overview

The Tonga National Portal is built on WordPress, a robust and flexible content management system that powers over 40% of websites worldwide. The platform leverages WordPress's core functionality while extending it with custom features to meet the specific needs of Tonga's government services portal.

### Core Architecture

#### Overview for Non-Technical Users

The Tonga National Portal uses WordPress as its foundation, which can be thought of as a digital framework that manages all the content and functionality of the website. Think of WordPress as the engine and chassis of a car, while the custom features and design elements are like the body, interior, and special features that make it uniquely suited for Tonga's government services.

The WordPress architecture consists of several key components:

1. **Core System**: The fundamental WordPress software that provides basic functionality
2. **Theme**: The Bricks Builder theme that controls how the portal looks and behaves
3. **Plugins**: Additional software components that extend functionality
4. **Database**: Where all content and settings are stored
5. **Media Library**: Storage system for images, documents, and other files

This architecture allows for:

- **Ease of Content Management**: Government staff can easily update content without technical knowledge
- **Flexibility**: The portal can be extended with new features as needs evolve
- **Security**: Regular updates to protect against vulnerabilities
- **Scalability**: The ability to handle increasing amounts of content and traffic

#### Technical Implementation

From a technical perspective, the WordPress architecture follows the Model-View-Controller (MVC) pattern, although with some WordPress-specific adaptations:

- **Model**: WordPress's database abstraction layer and core functions that interact with the database
- **View**: Theme templates and Bricks Builder elements that render the front-end interface
- **Controller**: WordPress core functions, hooks system, and custom PHP code that process requests

The request lifecycle in WordPress follows this general flow:

1. A user requests a URL on the Tonga National Portal
2. WordPress's core routing system determines what content to display
3. The appropriate template files from the theme are loaded
4. Hooks and filters modify and enhance the content
5. The final HTML is rendered and sent to the user's browser

```php
// Simplified example of WordPress request handling
function custom_tnp_init() {
    // Register custom post types, taxonomies, and other features
    register_post_type('government_service', [
        'public' => true,
        'label' => 'Government Services',
        // Additional configuration...
    ]);
    
    // Add other initialization code
}
add_action('init', 'custom_tnp_init');
```

### Extension Architecture

The Tonga National Portal extends the core WordPress architecture through:

1. **Custom Post Types**: Specialized content types for government services, departments, forms, etc.
2. **Custom Taxonomies**: Classification systems for organizing content
3. **Custom Fields**: Additional data associated with content
4. **Custom Templates**: Specialized layouts for different types of content
5. **Custom Endpoints**: API endpoints for integration with other systems

This extension architecture allows the portal to serve as a comprehensive government services platform while maintaining the benefits of the WordPress ecosystem.

## Database Structure

WordPress uses a relational database (MySQL 8.0) with a specific schema that has been extended for the Tonga National Portal's needs.

### Core WordPress Tables

The standard WordPress database includes these core tables:

1. **wp_posts**: Stores all content including pages, posts, attachments, and custom post types
2. **wp_users**: Contains user account information
3. **wp_usermeta**: Stores additional user metadata
4. **wp_comments**: Holds comments and comment metadata
5. **wp_terms**, **wp_term_taxonomy**, **wp_term_relationships**: Manage categories, tags, and custom taxonomies
6. **wp_options**: Stores site configuration settings
7. **wp_postmeta**: Contains additional metadata for posts and pages

### Custom Tables

The Tonga National Portal extends the database with additional custom tables:

1. **wp_tnp_services**: Tracks government service usage and statistics
2. **wp_tnp_form_submissions**: Stores form submissions from citizens
3. **wp_tnp_departments**: Contains detailed information about government departments
4. **wp_tnp_audit_log**: Records system activities for security and compliance

### Database Relationships

The database uses several key relationships to connect different types of content:

```
┌─────────────┐       ┌───────────────────┐       ┌────────────────┐
│  wp_users   │◄─────►│    wp_usermeta    │       │    wp_posts    │
└─────────────┘       └───────────────────┘       └────────┬───────┘
                                                          │
                                                          │
┌─────────────┐       ┌───────────────────┐       ┌───────▼───────┐
│  wp_terms   │◄─────►│ wp_term_taxonomy  │◄─────►│wp_term_relation│
└─────────────┘       └───────────────────┘       └────────────────┘
                                                          ▲
                                                          │
┌─────────────┐       ┌───────────────────┐       ┌───────┴───────┐
│ wp_options  │       │    wp_postmeta    │◄─────►│   wp_posts    │
└─────────────┘       └───────────────────┘       └────────────────┘
```

### Technical Details

The database uses InnoDB as the storage engine, with UTF-8mb4 character encoding to support multilingual content (both English and Tongan). Indexes are created on frequently queried columns to optimize performance.

Example of a custom table structure:

```sql
CREATE TABLE `wp_tnp_form_submissions` (
  `id` bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT,
  `form_id` bigint(20) UNSIGNED NOT NULL,
  `user_id` bigint(20) UNSIGNED DEFAULT NULL,
  `submission_data` longtext CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_520_ci NOT NULL,
  `submission_date` datetime NOT NULL DEFAULT current_timestamp(),
  `status` varchar(50) NOT NULL DEFAULT 'pending',
  `ip_address` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `form_id` (`form_id`),
  KEY `user_id` (`user_id`),
  KEY `status` (`status`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_520_ci;
```

## File System Organization

The Tonga National Portal follows WordPress's standard file system organization with additional custom directories and files.

### Core Directory Structure

```
tonga-national-portal/
├── wp-admin/                 # WordPress admin interface files
├── wp-content/               # Content files (themes, plugins, uploads)
│   ├── themes/               # WordPress themes
│   │   ├── tonga-portal/     # Custom theme for the portal
│   │   └── bricks/           # Bricks Builder parent theme
│   ├── plugins/              # WordPress plugins
│   │   ├── custom-tnp-core/  # Core functionality plugin for the portal
│   │   ├── bricks-builder/   # Bricks Builder plugin
│   │   └── [other plugins]/  # Additional functionality plugins
│   ├── uploads/              # Uploaded media files
│   │   ├── 2024/             # Files organized by year
│   │   └── 2025/             # Files organized by year
│   ├── languages/            # Translation files
│   └── mu-plugins/           # Must-use plugins
├── wp-includes/              # WordPress core files
└── [WordPress core files]    # Various WordPress system files
```

### Custom Theme Structure

The custom theme for the Tonga National Portal follows a modular organization:

```
tonga-portal/
├── assets/                   # Static assets
│   ├── css/                  # Stylesheets
│   ├── js/                   # JavaScript files
│   ├── fonts/                # Custom fonts
│   └── images/               # Theme images
├── inc/                      # PHP includes
│   ├── custom-post-types.php # Custom post type definitions
│   ├── taxonomies.php        # Custom taxonomy definitions
│   ├── shortcodes.php        # Shortcode definitions
│   ├── widgets.php           # Custom widget definitions
│   └── helpers.php           # Helper functions
├── template-parts/           # Reusable template parts
│   ├── content/              # Content templates
│   ├── header/               # Header templates
│   └── footer/               # Footer templates
├── templates/                # Page templates
├── languages/                # Translation files
├── functions.php             # Theme functions
├── style.css                 # Theme stylesheet
└── [other theme files]       # Various theme files
```

### Custom Plugin Structure

The core functionality plugin for the Tonga National Portal follows a modular, object-oriented structure:

```
custom-tnp-core/
├── admin/                    # Admin-specific functionality
├── includes/                 # Core functionality
│   ├── class-tnp-core.php    # Main plugin class
│   ├── class-tnp-services.php # Government services functionality
│   ├── class-tnp-forms.php   # Forms functionality
│   └── class-tnp-api.php     # API integration functionality
├── public/                   # Public-facing functionality
├── languages/                # Translation files
├── custom-tnp-core.php       # Main plugin file
└── uninstall.php             # Cleanup on uninstall
```

## Theme Structure Using Bricks Template Builder

The Tonga National Portal uses the Bricks Builder as its template builder, providing a flexible and powerful way to create and manage the portal's design and layout.

### Bricks Builder Overview

Bricks Builder is a visual page builder for WordPress that allows for the creation of complex layouts without writing code. It uses a modular approach with elements that can be combined to create pages.

#### Key Components

1. **Templates**: Pre-designed layouts that can be applied to different types of content
2. **Sections**: Reusable layout blocks that can be included in multiple pages
3. **Elements**: Individual components like text, images, buttons, etc.
4. **Global Settings**: Site-wide design settings for consistency

### Custom Bricks Templates

The Tonga National Portal uses several custom Bricks templates:

1. **Ministry Template**: Used for ministry and department pages
2. **Service Template**: Used for government service pages
3. **Form Template**: Used for online form pages
4. **News Template**: Used for news and announcements
5. **Document Template**: Used for document repository pages

### Template Hierarchy

The template hierarchy determines which template is used for different types of content:

```
┌─────────────────┐
│  Default Template │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Content Type   │
│    Templates    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Taxonomy/Term  │
│    Templates    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Individual Page │
│    Templates    │
└─────────────────┘
```

### Technical Implementation

Bricks templates are stored in the WordPress database and can be exported/imported as JSON files. The templates use a combination of HTML, CSS, and JavaScript, with dynamic content inserted through WordPress functions and custom code.

Example of a Bricks template structure (simplified JSON):

```json
{
  "name": "Ministry Template",
  "settings": {
    "width": "1200px",
    "margin": "0 auto"
  },
  "elements": [
    {
      "name": "header",
      "settings": {
        "background": "#00205b",
        "padding": "20px"
      },
      "children": [
        {
          "name": "logo",
          "settings": {
            "src": "{{logo_url}}",
            "alt": "Ministry Logo"
          }
        },
        {
          "name": "navigation",
          "settings": {
            "items": "{{menu_items}}"
          }
        }
      ]
    },
    {
      "name": "content",
      "settings": {
        "padding": "40px 20px"
      },
      "dynamic": "{{post_content}}"
    },
    {
      "name": "footer",
      "settings": {
        "background": "#f5f5f5",
        "padding": "30px"
      },
      "children": [
        {
          "name": "copyright",
          "content": "© {{current_year}} Government of Tonga"
        },
        {
          "name": "footer_menu",
          "settings": {
            "items": "{{footer_menu_items}}"
          }
        }
      ]
    }
  ]
}
```

## Custom Post Types, Taxonomies, and Other WordPress Elements

The Tonga National Portal extends WordPress's content management capabilities through custom post types, taxonomies, and other elements.

### Custom Post Types

Custom post types allow for specialized content types beyond the standard WordPress posts and pages. The portal includes these custom post types:

1. **Government Services**: For government service information and online applications
2. **Departments**: For ministry and department information
3. **Documents**: For official documents and publications
4. **Forms**: For online forms and applications
5. **Events**: For government events and important dates
6. **Announcements**: For official announcements and notices

Example registration of a custom post type:

```php
function tnp_register_post_types() {
    register_post_type('government_service', [
        'labels' => [
            'name' => __('Government Services', 'tnp'),
            'singular_name' => __('Government Service', 'tnp'),
        ],
        'public' => true,
        'has_archive' => true,
        'supports' => ['title', 'editor', 'thumbnail', 'excerpt', 'custom-fields'],
        'menu_icon' => 'dashicons-clipboard',
        'rewrite' => ['slug' => 'services'],
        'show_in_rest' => true,
    ]);
    
    // Additional post type registrations...
}
add_action('init', 'tnp_register_post_types');
```

### Custom Taxonomies

Custom taxonomies provide ways to categorize and organize content. The portal includes these custom taxonomies:

1. **Service Categories**: For categorizing government services
2. **Ministries**: For organizing content by government ministry
3. **Document Types**: For categorizing documents
4. **Audience**: For targeting content to specific user groups (citizens, businesses, visitors)
5. **Regions**: For organizing content by geographical region

Example registration of a custom taxonomy:

```php
function tnp_register_taxonomies() {
    register_taxonomy('service_category', ['government_service'], [
        'labels' => [
            'name' => __('Service Categories', 'tnp'),
            'singular_name' => __('Service Category', 'tnp'),
        ],
        'hierarchical' => true,
        'show_admin_column' => true,
        'rewrite' => ['slug' => 'service-category'],
        'show_in_rest' => true,
    ]);
    
    // Additional taxonomy registrations...
}
add_action('init', 'tnp_register_taxonomies');
```

### Custom Fields

Custom fields provide additional metadata for content. The portal uses Advanced Custom Fields (ACF) to implement these custom fields:

1. **Service Details**: Additional information for government services
   - Processing Time
   - Required Documents
   - Fees
   - Contact Information
   
2. **Department Details**: Additional information for government departments
   - Location
   - Contact Information
   - Opening Hours
   - Leadership

3. **Document Metadata**: Additional information for documents
   - Publication Date
   - Document Number
   - Related Legislation
   - File Type and Size

Example ACF field group:

```php
acf_add_local_field_group([
    'key' => 'group_service_details',
    'title' => 'Service Details',
    'fields' => [
        [
            'key' => 'field_processing_time',
            'label' => 'Processing Time',
            'name' => 'processing_time',
            'type' => 'text',
        ],
        [
            'key' => 'field_required_documents',
            'label' => 'Required Documents',
            'name' => 'required_documents',
            'type' => 'repeater',
            'sub_fields' => [
                [
                    'key' => 'field_document_name',
                    'label' => 'Document Name',
                    'name' => 'document_name',
                    'type' => 'text',
                ],
                [
                    'key' => 'field_document_description',
                    'label' => 'Description',
                    'name' => 'document_description',
                    'type' => 'textarea',
                ],
            ],
        ],
        // Additional fields...
    ],
    'location' => [
        [
            [
                'param' => 'post_type',
                'operator' => '==',
                'value' => 'government_service',
            ],
        ],
    ],
]);
```

### Custom Shortcodes

Shortcodes provide a way to insert dynamic content into pages and posts. The portal includes these custom shortcodes:

1. **[service_finder]**: Displays a searchable directory of government services
2. **[document_repository]**: Displays a searchable document repository
3. **[contact_directory]**: Displays a directory of government contacts
4. **[event_calendar]**: Displays an interactive calendar of government events

Example shortcode implementation:

```php
function tnp_service_finder_shortcode($atts) {
    $atts = shortcode_atts([
        'category' => '',
        'ministry' => '',
        'limit' => 10,
    ], $atts);
    
    // Query services based on attributes
    $args = [
        'post_type' => 'government_service',
        'posts_per_page' => $atts['limit'],
    ];
    
    if (!empty($atts['category'])) {
        $args['tax_query'][] = [
            'taxonomy' => 'service_category',
            'field' => 'slug',
            'terms' => $atts['category'],
        ];
    }
    
    if (!empty($atts['ministry'])) {
        $args['tax_query'][] = [
            'taxonomy' => 'ministry',
            'field' => 'slug',
            'terms' => $atts['ministry'],
        ];
    }
    
    $services = new WP_Query($args);
    
    // Generate and return HTML output
    ob_start();
    include(plugin_dir_path(__FILE__) . 'templates/service-finder.php');
    return ob_get_clean();
}
add_shortcode('service_finder', 'tnp_service_finder_shortcode');
```

### Custom Widgets

Widgets provide modular content blocks for sidebars and widget areas. The portal includes these custom widgets:

1. **Featured Services**: Displays featured government services
2. **Recent Announcements**: Displays recent announcements
3. **Quick Links**: Displays customizable quick links
4. **Contact Information**: Displays ministry contact information

Example widget implementation:

```php
class TNP_Featured_Services_Widget extends WP_Widget {
    public function __construct() {
        parent::__construct(
            'tnp_featured_services',
            __('Featured Services', 'tnp'),
            ['description' => __('Displays featured government services', 'tnp')]
        );
    }
    
    public function widget($args, $instance) {
        // Widget output code
    }
    
    public function form($instance) {
        // Widget admin form
    }
    
    public function update($new_instance, $old_instance) {
        // Process widget options
    }
}

function tnp_register_widgets() {
    register_widget('TNP_Featured_Services_Widget');
    // Register additional widgets...
}
add_action('widgets_init', 'tnp_register_widgets');
```

## Programming Methodology

The Tonga National Portal uses a combination of programming methodologies to balance WordPress conventions with modern development practices.

### Object-Oriented Programming (OOP)

The custom plugins for the portal use OOP principles to organize code into classes with specific responsibilities:

```php
class TNP_Service_Manager {
    private $post_type = 'government_service';
    
    public function __construct() {
        add_action('init', [$this, 'register_post_type']);
        add_action('add_meta_boxes', [$this, 'add_service_meta_boxes']);
        add_action('save_post', [$this, 'save_service_meta']);
    }
    
    public function register_post_type() {
        // Registration code
    }
    
    public function add_service_meta_boxes() {
        // Meta box code
    }
    
    public function save_service_meta($post_id) {
        // Save meta data code
    }
    
    // Additional methods...
}

// Initialize the class
new TNP_Service_Manager();
```

### Functional Programming

WordPress's hook system encourages a functional programming approach, which is used for many aspects of the portal:

```php
// Filter to modify service titles
function tnp_modify_service_title($title, $post_id) {
    if (get_post_type($post_id) === 'government_service') {
        $ministry = get_the_terms($post_id, 'ministry');
        if ($ministry && !is_wp_error($ministry)) {
            $title = $ministry[0]->name . ': ' . $title;
        }
    }
    return $title;
}
add_filter('the_title', 'tnp_modify_service_title', 10, 2);
```

### WordPress Coding Standards

The portal follows WordPress coding standards for consistency and compatibility:

- PHP code follows the WordPress PHP Coding Standards
- JavaScript code follows the WordPress JavaScript Coding Standards
- CSS follows the WordPress CSS Coding Standards
- Accessibility follows WCAG 2.1 AA standards

### Modular Development

The portal uses a modular development approach:

1. **Core Plugin**: Contains essential functionality
2. **Feature Plugins**: Add specific features that can be enabled/disabled
3. **Theme**: Handles presentation and layout
4. **Bricks Templates**: Define reusable layout components

This modular approach allows for:

- **Easier Maintenance**: Components can be updated independently
- **Feature Toggling**: Features can be enabled/disabled as needed
- **Code Reuse**: Common functionality is centralized
- **Separation of Concerns**: Code is organized by responsibility

## Component Interaction Diagrams

### System Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                  User's Web Browser                      │
└───────────────────────────┬─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                     Web Server                           │
│                                                         │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐    │
│  │   WordPress │   │    PHP      │   │   MySQL     │    │
│  │     Core    │◄──►│  Processing │◄──►│  Database  │    │
│  │             │   │             │   │             │    │
│  └─────────────┘   └─────────────┘   └─────────────┘    │
│                                                         │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐    │
│  │    Theme    │   │   Plugins   │   │   Media     │    │
│  │  (Bricks)   │   │             │   │   Files     │    │
│  │             │   │             │   │             │    │
│  └─────────────┘   └─────────────┘   └─────────────┘    │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Content Flow Diagram

```
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│  Admin User   │     │  WordPress    │     │   Database    │
│  Interface    │────►│  Processing   │────►│   Storage     │
└───────────────┘     └───────────────┘     └───────────────┘
                                                   │
                                                   ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│  Public User  │◄────│  WordPress    │◄────│  Template     │
│  Interface    │     │  Rendering    │     │  System       │
└───────────────┘     └───────────────┘     └───────────────┘
```

### Plugin Interaction Diagram

```
┌───────────────────────────────────────────────────────────┐
│                    WordPress Core                          │
└───────────┬───────────────────────────────┬───────────────┘
            │                               │
            ▼                               ▼
┌───────────────────┐             ┌───────────────────┐
│  Custom TNP Core  │◄───────────►│   Bricks Builder  │
│      Plugin       │             │      Plugin       │
└─────────┬─────────┘             └─────────┬─────────┘
          │                                 │
          ▼                                 ▼
┌───────────────────┐             ┌───────────────────┐
│  Feature Plugins  │◄───────────►│   Tonga Portal   │
│                   │             │      Theme        │
└───────────────────┘             └───────────────────┘
```

### Data Relationship Diagram

```
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│    Users      │────►│   User Roles  │     │  User Meta    │
└───────┬───────┘     └───────────────┘     └───────┬───────┘
        │                                           │
        └───────────────────┬───────────────────────┘
                            │
                            ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│    Posts      │────►│  Post Meta    │     │ Taxonomies    │
│  (All Types)  │     │               │     │ & Terms       │
└───────┬───────┘     └───────────────┘     └───────┬───────┘
        │                                           │
        └───────────────────┬───────────────────────┘
                            │
                            ▼
┌───────────────┐     ┌───────────────┐     ┌───────────────┐
│    Media      │     │  Comments     │     │   Options     │
│   Library     │     │               │     │               │
└───────────────┘     └───────────────┘     └───────────────┘
```

## Conclusion

The Tonga National Portal leverages WordPress's flexible architecture while extending it with custom features to meet the specific needs of a government services platform. The combination of WordPress core, Bricks Builder, custom post types, and custom plugins creates a powerful and maintainable platform that can evolve to meet the changing needs of Tonga's digital government initiatives.

The modular architecture ensures that:

1. Content can be easily managed by non-technical staff
2. New features can be added without disrupting existing functionality
3. The platform can scale to accommodate increasing content and traffic
4. Security can be maintained through regular updates
5. The user experience remains consistent across different sections of the portal

This platform structure provides a solid foundation for the Tonga National Portal's current functionality and future growth.