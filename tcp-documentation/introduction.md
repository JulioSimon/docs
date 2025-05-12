# Introduction

## Platform Purpose and Overview

The Citizen Portal is a comprehensive digital platform designed to streamline interactions between government entities and citizens. Built on modern web technologies, this platform serves as a centralized hub for various government services, enabling efficient management of administrative tasks, citizen requests, and public information.

The platform addresses several key challenges in government administration:

- **Fragmented Service Delivery**: Consolidates various government services into a single, unified platform
- **Administrative Inefficiency**: Automates routine tasks and streamlines workflows
- **Limited Accessibility**: Provides 24/7 online access to government services
- **Communication Gaps**: Facilitates direct communication between citizens and government officials
- **Resource Management**: Optimizes allocation and utilization of government resources

## Technology Stack

The platform is built using a robust and modern technology stack:

| Component | Technology | Version |
|-----------|------------|---------|
| Backend Framework | Laravel | 11.x |
| Admin Panel | FilamentPHP | 3.2.x |
| UI Interactivity | Livewire | 3.x |
| Database | MariaDB | 10.x |
| Authentication | Laravel Sanctum & FilamentBreezy | 4.0.x & 2.4.x |
| Authorization | Spatie Permissions & FilamentShield | 3.2.x |
| File Storage | Spatie Media Library | 3.2.x |
| UI Framework | TailwindCSS | 3.x |
| Icons | Heroicons | 2.x |
| Calendar | Guava Calendar | 1.10.x |
| API Documentation | Rupadana API Service | 3.4.x |

This stack provides a balance of developer productivity, performance, and maintainability while leveraging the ecosystem of Laravel and FilamentPHP.

## Panel Descriptions

The platform consists of two distinct panels, each serving different user groups:

### Admin Panel

The Admin Panel is designed for government staff and administrators. It provides a comprehensive set of tools for managing government operations, citizen requests, and internal workflows. Key features include:

- **Tenant-Based Architecture**: Each government ministry (entity) operates within its own isolated environment
- **Role-Based Access Control**: Different permission levels for administrators, managers, and staff
- **Comprehensive Dashboard**: Real-time analytics and insights into government operations
- **Advanced Management Tools**: For appointments, tickets, documents, vacancies, and e-services

### App Panel

The App Panel serves as the citizen-facing interface of the platform. It provides a user-friendly experience for accessing government services, submitting requests, and tracking their status. Key features include:

- **Personalized Dashboard**: Displays information relevant to the logged-in citizen
- **Service Request System**: For submitting and tracking various government service requests
- **Document Management**: Access to personal documents and government forms
- **Appointment Booking**: Schedule meetings with government officials
- **Job Application System**: Apply for government job vacancies

## System Architecture Overview

The platform follows a multi-tenant architecture with a clear separation between the Admin and App panels:

```
┌─────────────────────────────────────────────────────────┐
│                  Government Portal                       │
├───────────────────────────┬─────────────────────────────┤
│        Admin Panel        │         App Panel           │
│    (Government Staff)     │         (Citizens)          │
├───────────────────────────┼─────────────────────────────┤
│                                                         │
│                    Shared Services                       │
│  ┌─────────┐ ┌──────────┐ ┌───────────┐ ┌───────────┐   │
│  │ Auth &  │ │ Storage  │ │ Notifi-   │ │ Logging & │   │
│  │ Perms   │ │ Service  │ │ cations   │ │ Auditing  │   │
│  └─────────┘ └──────────┘ └───────────┘ └───────────┘   │
│                                                         │
├─────────────────────────────────────────────────────────┤
│                                                         │
│                    Core Modules                          │
│  ┌─────────┐ ┌──────────┐ ┌───────────┐ ┌───────────┐   │
│  │ Ticket  │ │Appointment│ │ Document  │ │ Vacancy   │   │
│  │ System  │ │ System    │ │ Management│ │ System    │   │
│  └─────────┘ └──────────┘ └───────────┘ └───────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │              E-Services System                   │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

The system uses Laravel's robust ORM for database interactions and follows the MVC (Model-View-Controller) pattern. FilamentPHP provides the admin panel framework, while Livewire enables dynamic, reactive user interfaces without complex JavaScript.

## Key Features Summary

### For Government Staff (Admin Panel)

1. **Entity Management**: Create and manage government ministries and departments
2. **User Management**: Administer staff accounts, roles, and permissions
3. **Ticket System**: Handle citizen inquiries and service requests
4. **Appointment Management**: Schedule and manage meetings with citizens
5. **Document Library**: Organize and share official documents
6. **Vacancy Management**: Post job openings and process applications
7. **E-Services Administration**: Configure and manage online government services
8. **Analytics Dashboard**: Monitor key performance indicators and service metrics

### For Citizens (App Panel)

1. **Service Requests**: Submit and track various government service requests
2. **Appointment Booking**: Schedule meetings with government officials
3. **Document Access**: View and download personal documents and forms
4. **Job Applications**: Apply for government job vacancies
5. **E-Services**: Access and use online government services
6. **Profile Management**: Update personal information and preferences
7. **Notifications**: Receive updates on request status and important announcements

This platform represents a significant step forward in digital government services, providing a seamless experience for both government staff and citizens while ensuring security, efficiency, and accessibility.
