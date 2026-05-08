# ✈️ Voyager Travel Agency — ASP.NET Core MVC 8

A complete, production-ready travel agency web application built with ASP.NET Core MVC 8,
Entity Framework Core, SQL Server, and a modern Bootstrap 5 UI.

---

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Setup & Run Instructions](#setup--run-instructions)
- [Default Credentials](#default-credentials)
- [Database Commands](#database-commands)
- [Pages & Routes](#pages--routes)

---

## ✅ Features

### Public Pages
| Page | URL | Description |
|------|-----|-------------|
| Home | `/` | Hero banner, search bar, featured tours, stats, testimonials |
| Tours | `/Tours` | Full tour listing with search, category & price filters |
| Tour Details | `/Tours/Details/{id}` | Full tour info, booking sidebar, related tours |
| Book Tour | `/Booking/Create?tourId={id}` | Booking form with live price calculation |
| Confirmation | `/Booking/Confirmation/{id}` | Booking confirmation page |
| About | `/Home/About` | Company story, values, team |
| Contact | `/Home/Contact` | Contact form |
| Register | `/Account/Register` | User registration |
| Login | `/Account/Login` | User login |

### Admin Panel (`/Admin` — requires Admin role)
| Page | Description |
|------|-------------|
| Dashboard | Stats overview, recent bookings, quick actions |
| Tours | List, create, edit, delete tours |
| Bookings | View all bookings, update status |
| Messages | Read contact form submissions |

---

## 🛠 Tech Stack

- **Framework**: ASP.NET Core MVC 8.0
- **ORM**: Entity Framework Core 8.0
- **Database**: SQL Server (LocalDB for development)
- **Identity**: ASP.NET Core Identity
- **UI**: Bootstrap 5.3, Font Awesome 6.5
- **Fonts**: Playfair Display + DM Sans (Google Fonts)
- **Validation**: jQuery Unobtrusive Validation

---

## 📁 Project Structure

```
TravelAgency/
├── Controllers/
│   ├── HomeController.cs        # Home, About, Contact
│   ├── ToursController.cs       # Tour listing & details
│   ├── BookingController.cs     # Booking form & confirmation
│   ├── AccountController.cs     # Register, Login, Logout
│   └── AdminController.cs       # Full admin panel (role-protected)
├── Models/
│   ├── Tour.cs
│   ├── Booking.cs
│   ├── ContactMessage.cs
│   └── ApplicationUser.cs
├── ViewModels/
│   └── ViewModels.cs            # All view models in one file
├── Data/
│   └── ApplicationDbContext.cs  # EF Core context + seed data
├── Views/
│   ├── Shared/
│   │   ├── _Layout.cshtml       # Main site layout
│   │   ├── _AdminLayout.cshtml  # Admin panel layout
│   │   └── _ValidationScriptsPartial.cshtml
│   ├── Home/        (Index, About, Contact, Error)
│   ├── Tours/       (Index, Details)
│   ├── Booking/     (Create, Confirmation)
│   ├── Account/     (Login, Register)
│   └── Admin/       (Index, Tours, CreateTour, EditTour, Bookings, Messages)
├── wwwroot/
│   ├── css/
│   │   ├── site.css             # Main stylesheet
│   │   └── admin.css            # Admin panel styles
│   └── js/
│       └── site.js              # Client-side JS
├── Program.cs                   # App startup, DI, seeding
├── appsettings.json
└── TravelAgency.csproj
```

---

## 📦 Prerequisites

1. [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
2. [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
   or SQL Server LocalDB (included with Visual Studio)
3. [Visual Studio 2022](https://visualstudio.microsoft.com/) or [VS Code](https://code.visualstudio.com/)
4. EF Core CLI tools (see below)

---

## 🚀 Setup & Run Instructions

### Step 1 — Clone / Extract the project

```bash
cd TravelAgency
```

### Step 2 — Install EF Core CLI tools (one-time)

```bash
dotnet tool install --global dotnet-ef
```

### Step 3 — Restore NuGet packages

```bash
dotnet restore
```

### Step 4 — Configure the database connection

Edit `appsettings.json` and set your connection string:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=TravelAgencyDb;Trusted_Connection=True;MultipleActiveResultSets=true"
}
```

For a full SQL Server instance:
```json
"DefaultConnection": "Server=YOUR_SERVER;Database=TravelAgencyDb;User Id=sa;Password=YOUR_PASSWORD;TrustServerCertificate=True"
```

### Step 5 — Create and apply the database migration

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

> **Note:** `Program.cs` also calls `db.Database.Migrate()` on startup, so
> the database will be created automatically if it doesn't exist when you run the app.

### Step 6 — Run the application

```bash
dotnet run
```

Then open your browser at: **https://localhost:5001** (or the port shown in the terminal)

---

## 🔑 Default Credentials

| Role | Email | Password |
|------|-------|----------|
| **Admin** | admin@travelagency.com | Admin@123 |
| **User** | Register via `/Account/Register` | Your choice |

The admin account and roles are seeded automatically on first run.

---

## 🗄 Database Commands Reference

```bash
# Create a new migration
dotnet ef migrations add MigrationName

# Apply all pending migrations
dotnet ef database update

# Rollback to a specific migration
dotnet ef database update PreviousMigrationName

# Drop the database (WARNING: deletes all data)
dotnet ef database drop

# List all migrations
dotnet ef migrations list

# Remove last migration (if not yet applied)
dotnet ef migrations remove
```

---

## 🗺 Pages & Routes

```
GET  /                              → Home page
GET  /Tours                         → All tours (with filters)
GET  /Tours?search=Bali             → Search tours
GET  /Tours?category=Beach          → Filter by category
GET  /Tours?minPrice=500&maxPrice=2000 → Filter by price
GET  /Tours/Details/{id}            → Tour detail page
GET  /Booking/Create?tourId={id}    → Booking form
POST /Booking/Create                → Submit booking
GET  /Booking/Confirmation/{id}     → Booking confirmed

GET  /Home/About                    → About Us
GET  /Home/Contact                  → Contact page
POST /Home/Contact                  → Submit contact form

GET  /Account/Register              → Register form
POST /Account/Register              → Create account
GET  /Account/Login                 → Login form
POST /Account/Login                 → Authenticate
POST /Account/Logout                → Sign out

GET  /Admin                         → Dashboard (Admin only)
GET  /Admin/Tours                   → Manage tours
GET  /Admin/CreateTour              → Add tour form
POST /Admin/CreateTour              → Save new tour
GET  /Admin/EditTour/{id}           → Edit tour form
POST /Admin/EditTour                → Update tour
POST /Admin/DeleteTour              → Delete tour
GET  /Admin/Bookings                → All bookings
POST /Admin/UpdateBookingStatus     → Change booking status
GET  /Admin/Messages                → Contact messages
POST /Admin/MarkMessageRead         → Mark message read
```

---

## 🎨 Design Highlights

- **Color palette**: Deep navy (#0f1e3c), warm gold (#c9a84c), soft ivory (#faf8f3)
- **Typography**: Playfair Display (headings) + DM Sans (body)
- **Effects**: Hero parallax, card hover lift, scroll-reveal animations, smooth transitions
- **Fully responsive**: Mobile-first Bootstrap 5 grid throughout
- **Accessibility**: Proper ARIA labels, semantic HTML, keyboard-navigable forms

---

## 🔒 Security Features

- ASP.NET Core Identity password hashing
- Anti-forgery tokens on all POST forms
- Role-based authorization (`[Authorize(Roles = "Admin")]`)
- Model validation (server-side + client-side)
- Input sanitization via model binding

---

## 📝 Sample Seed Data

The database is pre-seeded with 6 tours:

| # | Title | Destination | Price | Category |
|---|-------|-------------|-------|----------|
| 1 | Maldives Paradise Escape | Maldives | $2,499 | Beach |
| 2 | Swiss Alps Adventure | Switzerland | $3,299 | Adventure |
| 3 | Bali Cultural Journey | Bali, Indonesia | $1,799 | Culture |
| 4 | Santorini Sunset Cruise | Santorini, Greece | $2,199 | Luxury |
| 5 | Safari in Kenya | Kenya, Africa | $4,199 | Adventure |
| 6 | Tokyo Tech & Tradition | Tokyo, Japan | $2,899 | Culture |

---

## 🐛 Troubleshooting

**"Cannot open database" error**
→ Make sure SQL Server / LocalDB is running. Run `sqllocaldb start MSSQLLocalDB`.

**"dotnet-ef not found"**
→ Run `dotnet tool install --global dotnet-ef` and restart your terminal.

**Migrations folder is empty**
→ Run `dotnet ef migrations add InitialCreate` to generate migration files.

**CSS/JS not loading**
→ Run `dotnet run` from the project root (not inside a sub-folder).

---

*Built with ❤️ using ASP.NET Core MVC 8 | Voyager Travel Agency*
