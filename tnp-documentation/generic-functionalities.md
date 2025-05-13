# Generic WordPress Functionalities

This document provides a comprehensive overview of the generic WordPress functionalities available in the Tonga National Portal installation. These core capabilities form the foundation of the portal's content management system and are extended by custom features specific to government services.

## Content Management Capabilities

WordPress provides robust content management capabilities that allow government staff to create, edit, and organize various types of content.

### Posts and Pages

#### Posts

Posts are dynamic content entries typically used for time-sensitive content such as:

- News articles
- Press releases
- Announcements
- Updates

Key features of posts include:

- **Categories and Tags**: For organizing and filtering content
- **Featured Images**: Visual representation of the content
- **Excerpts**: Brief summaries for listings and search results
- **Comments**: Optional user engagement feature
- **Revisions**: Track changes and restore previous versions

#### Pages

Pages are static content entries used for permanent or semi-permanent information such as:

- About sections
- Contact information
- Department descriptions
- Service details

Key features of pages include:

- **Hierarchical Structure**: Parent-child relationships between pages
- **Custom Templates**: Specialized layouts for different types of content
- **Order Attribute**: Control the display order in menus
- **Featured Images**: Visual elements to enhance content
- **Revisions**: Track changes and restore previous versions

### Content Editor

The Tonga National Portal uses the WordPress Block Editor (Gutenberg) enhanced with Bricks Builder for content creation and editing:

- **Block-Based Editing**: Content is created using modular blocks
- **Rich Text Formatting**: Text styling, links, tables, and more
- **Media Integration**: Easily embed images, videos, and documents
- **Reusable Blocks**: Save and reuse content across the portal
- **Custom Blocks**: Specialized blocks for government-specific content

![Screenshot: WordPress Block Editor with government content blocks]
(Here a screenshot would show the WordPress editor interface with custom blocks for government content)

### Content Organization

Content can be organized using:

- **Categories**: Hierarchical taxonomy for broad classification
- **Tags**: Non-hierarchical taxonomy for specific topics
- **Custom Taxonomies**: Specialized classification systems (e.g., Ministries, Services)
- **Custom Fields**: Additional metadata for enhanced content organization

## User Management and Role Capabilities

The Tonga National Portal implements WordPress's comprehensive user management system with role-based access control.

### User Roles

The portal uses the following standard WordPress roles with customized permissions:

1. **Super Administrator**: Complete access to all portal features and network settings
   - Manage network settings
   - Install and activate plugins and themes
   - Create and manage sites
   - Manage all user accounts

2. **Administrator**: Complete access to features and settings of a specific site
   - Manage site settings
   - Create and edit all content
   - Manage users
   - Install plugins and themes

3. **Editor**: Content management responsibilities
   - Create, edit, publish, and delete any content
   - Moderate comments
   - Manage categories, tags, and links
   - Cannot modify site settings or install plugins/themes

4. **Author**: Content creation with limited editing rights
   - Create, edit, publish, and delete their own content
   - Upload media files
   - Cannot modify others' content or site settings

5. **Contributor**: Limited content creation rights
   - Create and edit their own posts (but cannot publish)
   - Cannot upload media files
   - Cannot modify site settings or others' content

6. **Subscriber**: Basic access for registered users
   - Read content (including restricted content if applicable)
   - Manage their own profile
   - Comment on posts (if enabled)

### Custom Roles

In addition to standard roles, the Tonga National Portal implements custom roles specific to government operations:

1. **Ministry Manager**: Department-specific administrative capabilities
   - Manage content for specific ministry/department
   - Create and publish service information
   - Moderate comments on ministry content
   - Access ministry-specific analytics

2. **Service Manager**: Focused on government service content
   - Create and update service information
   - Process service requests
   - Generate service reports
   - Respond to service inquiries

3. **Content Approver**: Content workflow management
   - Review and approve content before publication
   - Provide feedback on content submissions
   - Ensure content meets government standards
   - Track content approval workflows

### User Management Features

Administrators can manage users through these features:

- **User Registration**: Control who can register and how
- **Profile Management**: User information and preferences
- **Role Assignment**: Assign appropriate access levels
- **Multi-site Capabilities**: Manage users across multiple sites
- **User Activity Tracking**: Monitor user actions for security

![Screenshot: User management interface]
(Here a screenshot would show the user management dashboard with custom roles)

## Comment Management

The comment system allows for public engagement and feedback on portal content.

### Comment Features

- **Moderation Queue**: Review comments before publication
- **Spam Protection**: Automatic filtering of spam comments
- **Threaded Comments**: Hierarchical discussion structure
- **Email Notifications**: Alerts for new comments
- **Comment Editing**: Administrative editing capabilities

### Comment Settings

Administrators can configure:

- **Comment Approval**: Automatic or manual approval requirements
- **Comment Registration**: Whether users must be registered to comment
- **Comment Notifications**: Who receives email alerts about new comments
- **Comment Moderation**: Triggers for automatic moderation (links, keywords)
- **Comment Blacklist**: Words, IPs, or emails that trigger automatic rejection

## Menu and Navigation Management

The Tonga National Portal uses WordPress's menu management system for creating and organizing navigation structures.

### Menu Creation and Management

- **Custom Menu Builder**: Drag-and-drop interface for menu creation
- **Multiple Menu Locations**: Different menus for different areas of the site
- **Hierarchical Menus**: Multi-level dropdown navigation
- **Custom Links**: Add external or internal links to menus
- **Menu Items Customization**: CSS classes, target attributes, and titles

### Navigation Features

- **Breadcrumb Navigation**: Show content hierarchy and location
- **Mobile-Responsive Menus**: Adapt to different screen sizes
- **Mega Menus**: Enhanced dropdown menus for complex navigation
- **Current Page Highlighting**: Visual indication of active page
- **Accessibility Features**: Keyboard navigation and screen reader support

![Screenshot: Menu management interface]
(Here a screenshot would show the menu management interface with government department structure)

## Widget and Sidebar Management

Widgets allow for flexible content placement in designated areas of the portal.

### Widget Areas

The Tonga National Portal includes these widget areas:

- **Main Sidebar**: Primary sidebar for most pages
- **Footer Widgets**: Multiple widget areas in the footer
- **Homepage Widgets**: Special widget areas for the homepage
- **Department Sidebars**: Custom sidebars for different departments
- **Service Sidebars**: Specialized widget areas for service pages

### Available Widgets

Standard WordPress widgets available include:

- **Text Widget**: Custom text or HTML content
- **Recent Posts**: Display recent content
- **Pages Widget**: List of pages
- **Calendar**: Date-based navigation
- **Categories**: List of content categories
- **Custom Menu**: Display a custom navigation menu
- **Search**: Search functionality
- **Media**: Images, audio, or video content

### Custom Widgets

The portal includes custom widgets specific to government needs:

- **Service Finder**: Quick access to government services
- **Document Browser**: Access to recent or important documents
- **Contact Information**: Ministry or department contact details
- **Announcement Ticker**: Scrolling or highlighted announcements
- **Event Calendar**: Upcoming government events
- **Social Media Feed**: Official government social media updates

## Theme Customization Options

The Tonga National Portal uses a custom theme built with Bricks Builder that offers extensive customization options.

### Theme Customizer

The WordPress Theme Customizer provides a visual interface for theme modifications:

- **Colors**: Primary, secondary, and accent color schemes
- **Typography**: Font families, sizes, and styles
- **Layout Options**: Content width, sidebar positions
- **Header Options**: Logo, navigation style, header layout
- **Footer Options**: Footer content, layout, and styling
- **Background**: Background colors or images

### Bricks Builder Customization

Bricks Builder extends theme customization with:

- **Visual Editing**: WYSIWYG interface for layout design
- **Template System**: Create and apply templates to different content types
- **Global Elements**: Reusable components across the site
- **Responsive Controls**: Device-specific layout adjustments
- **Custom CSS/JS**: Advanced styling and functionality

![Screenshot: Theme customization interface]
(Here a screenshot would show the theme customizer with Tonga government branding options)

## Media Library Management

The WordPress Media Library provides comprehensive tools for managing digital assets.

### Media Management Features

- **Centralized Repository**: Single location for all uploaded files
- **File Organization**: Organize media by date or custom folders
- **Search Functionality**: Find media by filename, date, or type
- **Bulk Selection**: Select and modify multiple files at once
- **Media Editing**: Basic image editing capabilities

### Supported Media Types

The portal supports various media types:

- **Images**: JPEG, PNG, GIF, WebP
- **Documents**: PDF, DOC/DOCX, XLS/XLSX, PPT/PPTX
- **Audio**: MP3, WAV, OGG
- **Video**: MP4, WebM
- **Other**: CSV, ZIP, and other file formats

### Media Upload and Integration

- **Drag-and-Drop Upload**: Easy file uploading
- **Media Insertion**: Insert media into content with a few clicks
- **Featured Images**: Assign representative images to content
- **Galleries**: Create collections of images
- **Attachment Pages**: Dedicated pages for media items
- **Responsive Images**: Automatic resizing for different devices

![Screenshot: Media library interface]
(Here a screenshot would show the media library with government documents and images)

## Search Functionality

The Tonga National Portal implements enhanced search capabilities to help users find government information quickly.

### Basic Search Features

- **Site-wide Search**: Search across all content types
- **Search Results Highlighting**: Highlight search terms in results
- **Relevance Sorting**: Display most relevant results first
- **Filter by Content Type**: Narrow results to specific content types
- **Search Analytics**: Track common search terms

### Enhanced Search Capabilities

The portal extends WordPress's search with:

- **Faceted Search**: Filter results by taxonomy, date, or custom fields
- **Live Search**: Real-time search suggestions as users type
- **Multilingual Search**: Search across both English and Tongan content
- **Document Content Search**: Search within PDF and document contents
- **Advanced Operators**: Support for AND, OR, NOT, and phrase searching

![Screenshot: Search results page]
(Here a screenshot would show search results for a government service)

## SEO Capabilities

The Tonga National Portal includes search engine optimization features to improve visibility and accessibility.

### Built-in SEO Features

WordPress provides these SEO capabilities:

- **SEO-friendly URLs**: Clean, readable permalinks
- **Customizable Page Titles**: Control how titles appear in search results
- **Meta Descriptions**: Custom descriptions for search engine listings
- **Image Alt Text**: Accessibility and SEO for images
- **XML Sitemaps**: Help search engines discover content

### Enhanced SEO Features

The portal extends SEO capabilities with:

- **Schema Markup**: Structured data for enhanced search listings
- **Social Media Integration**: Open Graph and Twitter Card support
- **SEO Analysis**: Content optimization suggestions
- **Redirect Management**: Handle URL changes and prevent broken links
- **Breadcrumb Navigation**: Improve site structure for search engines
- **Focus Keywords**: Target specific keywords for content

## Multilingual Support via WPML

The Tonga National Portal uses the WPML (WordPress Multilingual) plugin to provide content in both English and Tongan languages.

### WPML Features

- **Content Translation**: Translate posts, pages, custom post types, and taxonomies
- **Language Switcher**: Allow users to switch between available languages
- **Translation Management**: Workflow for managing translation tasks
- **String Translation**: Translate theme and plugin text
- **Media Translation**: Translate image captions and descriptions
- **SEO Translation**: Translate meta titles, descriptions, and keywords

### Translation Workflow

The portal implements this translation workflow:

1. **Content Creation**: Create content in the primary language (English)
2. **Translation Assignment**: Assign content for translation to Tongan
3. **Translation Process**: Translate content using the WPML interface
4. **Review and Approval**: Review translations for accuracy
5. **Publication**: Publish translated content
6. **Maintenance**: Update translations when original content changes

### Language Configuration

The portal is configured with:

- **Default Language**: English
- **Secondary Language**: Tongan
- **Language Detection**: Based on user preference or browser settings
- **URL Format**: Language code in URL (e.g., /to/ for Tongan pages)
- **Content Synchronization**: Keep content structure consistent across languages

![Screenshot: WPML translation interface]
(Here a screenshot would show the WPML translation interface with English to Tongan translation)

## Conclusion

The generic WordPress functionalities described in this document provide the foundation for the Tonga National Portal's content management capabilities. These features are extended and customized to meet the specific needs of government service delivery and citizen engagement. Government staff should familiarize themselves with these core capabilities to effectively manage and maintain the portal content.

For more information on custom functionalities specific to government services, please refer to the [Module Functionalities](./module-functionalities.md) documentation.