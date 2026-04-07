# GoldenEstate.homes - Real Estate Website

A modern, animated real estate website for luxury properties in Los Angeles, California.

## Features

- **Responsive Design**: Works perfectly on desktop, tablet, and mobile devices
- **Smooth Animations**: Engaging scroll animations and hover effects
- **Property Filtering**: Filter properties by category (All, Luxury, Modern, Villas)
- **Interactive Navigation**: Smooth scrolling navigation with active state indicators
- **Contact Form**: Functional contact form with validation
- **Statistics Counter**: Animated statistics that count up on scroll
- **Property Listings**: 6 featured luxury properties with details

## Technologies Used

- HTML5
- CSS3 (with animations and transitions)
- Vanilla JavaScript (no frameworks required)

## Setup Instructions

1. **Add Property Images**:
   - Create an `images` folder in the root directory
   - Add the following images:
     - `hero-bg.jpg` - Hero section background (1920x1080px recommended)
     - `home1.jpg` - Beverly Hills Mansion
     - `home2.jpg` - Hollywood Hills Contemporary
     - `home3.jpg` - Malibu Beach Villa
     - `home4.jpg` - Bel Air Estate
     - `home5.jpg` - Santa Monica Modern
     - `home6.jpg` - Pacific Palisades Villa
     - `about.jpg` - About section image

2. **Image Sources** (Free high-quality images):
   - [Unsplash](https://unsplash.com) - Search: "luxury home los angeles", "modern mansion"
   - [Pexels](https://pexels.com) - Search: "luxury real estate", "california mansion"
   - [Pixabay](https://pixabay.com) - Search: "luxury house"

3. **Open the Website**:
   - Simply open `index.html` in your web browser
   - No server required - it's a static website

## File Structure

```
GoldEstates/
├── index.html          # Main HTML file
├── styles.css          # All CSS styling and animations
├── script.js           # JavaScript functionality
├── README.md           # This file
└── images/             # Property images folder
    ├── hero-bg.jpg
    ├── home1.jpg
    ├── home2.jpg
    ├── home3.jpg
    ├── home4.jpg
    ├── home5.jpg
    ├── home6.jpg
    └── about.jpg
```

## Features Breakdown

### Navigation
- Fixed navigation bar with smooth scroll
- Active section highlighting
- Mobile-responsive hamburger menu

### Hero Section
- Full-screen hero with background image
- Parallax scrolling effect
- Call-to-action buttons
- Animated scroll indicator

### Statistics Section
- Animated counters that trigger on scroll
- 4 key metrics displayed

### Properties Section
- 6 luxury property listings
- Filter by category functionality
- Hover effects with "View Details" overlay
- Property cards with images, prices, and features

### About Section
- Company information
- Feature highlights with icons
- Image with hover zoom effect

### Contact Section
- Contact information cards
- Functional contact form
- Form validation and submission feedback

### Footer
- Quick links
- Services
- Social media links
- Copyright information

## Customization

### Colors
Edit the CSS variables in `styles.css`:
```css
:root {
    --primary-color: #d4af37;      /* Gold color */
    --secondary-color: #1a1a1a;    /* Dark background */
    --text-color: #333;            /* Text color */
    --light-bg: #f8f9fa;           /* Light background */
}
```

### Property Listings
Edit the property cards in `index.html` to add/modify listings:
- Update prices
- Change property names
- Modify features (beds, baths, sqft)
- Update locations

### Contact Information
Update contact details in the contact section:
- Phone number
- Email address
- Office address

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers

## Performance

- Optimized CSS animations
- Lazy loading for images
- Minimal JavaScript for fast load times
- No external dependencies

## Future Enhancements

Potential features to add:
- Property detail pages
- Image galleries/lightbox
- Virtual tours integration
- Search functionality
- Map integration
- Backend integration for form submissions
- Property comparison tool
- Mortgage calculator

## License

This website template is free to use for GoldenEstate.homes.

## Support

For questions or issues, contact: info@goldenestate.homes

---

**Note**: Remember to replace all placeholder images with actual property photos before going live!
