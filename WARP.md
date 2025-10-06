# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a React-based movie search application built with Create React App that uses the OMDB API to search and display movie, series, and game information. The app features pagination, filtering by content type, and responsive design.

## Development Commands

### Core Development
- `npm start` - Start development server at http://localhost:3000 with hot reload
- `npm test` - Run tests in interactive watch mode (using Jest/React Testing Library)
- `npm run build` - Create optimized production build in `build/` folder
- `npm run eject` - Eject from Create React App (one-way operation, use with caution)

### Deployment
- `npm run predeploy` - Automatically runs build before deployment
- `npm run deploy` - Deploy to GitHub Pages using gh-pages

### Running Single Tests
Since this uses Create React App's test runner:
- `npm test -- --testNamePattern="specific test name"` - Run specific test by name
- `npm test -- --watchAll=false` - Run tests once without watch mode
- `npm test -- --coverage` - Run tests with coverage report

## Architecture

### Component Structure
```
src/
├── App.js                    # Root component with layout structure
├── index.js                  # React app entry point
├── components/               # Reusable UI components
│   ├── Movie.js             # Individual movie card display
│   ├── MovieList.js         # Grid of movie cards with "Not Found" handling
│   ├── Search.js            # Search input, filters, and pagination controls
│   └── Loader.js            # Loading state component
└── layout/                  # Layout components
    ├── Header.js            # Site header
    ├── Main.js              # Main content area with API logic and state
    └── Footer.js            # Site footer
```

### State Management Pattern
- Uses React class components with local state (no Redux/Context API)
- Main state lives in `src/layout/Main.js` component:
  - `movies[]` - Current search results
  - `loading` - Loading state boolean
  - `totalResults` - Total count for pagination
  - `currentPage` - Active page number
  - `currentSearch` - Current search term
  - `currentType` - Filter type (all/movie/series/game)

### API Integration
- **External API**: OMDB API (http://www.omdbapi.com/) with hardcoded API key `ae546d65`
- **Search endpoint**: `/?apikey={key}&s={query}&type={type}&page={page}`
- **Error handling**: Basic try/catch with fallback to empty results
- **Rate limiting**: None implemented (consider adding for production)

### Key Architectural Decisions
- **Class components**: Uses legacy class component pattern throughout (not modern hooks)
- **Prop drilling**: State passed down through props rather than context
- **Inline styles**: Mix of CSS files and inline styling
- **Pagination**: Custom pagination logic with ellipsis for large result sets (shows up to 9 results per page)

## Development Notes

### API Key Management
The OMDB API key is hardcoded in `src/layout/Main.js` line 29. For production or team development:
- Move API key to environment variables using `.env` file
- Use `process.env.REACT_APP_OMDB_API_KEY` pattern (Create React App convention)

### Component Modernization Opportunities
- Components use class-based pattern - could be refactored to functional components with hooks
- State management could benefit from useContext or useReducer for complex state
- Search component has a typo: `handelFilter` should be `handleFilter`

### Styling Architecture
- Each component has corresponding CSS file (e.g., `Search.css`, `MovieList.css`)
- Global styles in `src/index.css`
- No CSS framework (pure CSS with custom styling)

### Testing Setup
- React Testing Library and Jest are configured
- No existing test files found - test coverage needs to be added
- Test files should follow `*.test.js` or `*.spec.js` naming convention

### Deployment Configuration
- Configured for GitHub Pages deployment
- `homepage` field in package.json needs to be updated with actual repository URL
- Build output goes to `build/` directory for static hosting

## Common Development Patterns

When working with this codebase:
- Follow existing class component patterns for consistency
- Add new components in appropriate `components/` or `layout/` directories
- Include corresponding CSS files for new components
- API calls should follow the existing fetch pattern with error handling
- State updates should use `this.setState()` with functional updates where needed