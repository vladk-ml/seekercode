# SEEKER FRAMEWORK DOCUMENTATION
Version: 0.3.0
Last Updated: 2024-11-09

## PROJECT OVERVIEW
Development tool for analyzing radar interference patterns in Sentinel-1 SAR (Synthetic Aperture Radar) data using Google Earth Engine API. The application leverages VSCode's UI framework, stripped of IDE-specific functionality while maintaining essential features, focusing on:
- Pattern detection in SAR imagery
- Time series analysis
- GeoTIFF processing and visualization
- AI-assisted analysis
- Terminal-based AI model integration
- Efficient data management and exploration

## DEVELOPMENT STRATEGY AND LICENSING

### Legal Framework and Approach
This project is built on a solid legal and ethical foundation:

1. Source Code Base:
- VSCode: MIT licensed, open-source project
- Core UI components freely available for modification
- Public repository with clear licensing

2. Microsoft Component Removal:
- VSCodium's proven build process
- Removes all proprietary Microsoft components
- Results in clean, MIT-licensed codebase
- Industry-standard approach (as demonstrated by VSCodium project)

3. Our Development Strategy:
- Start with MIT-licensed VSCode source
- Apply VSCodium patches to remove Microsoft property
- Create original radar analysis tool
- Add custom functionality
- Clear differentiation from VSCode/VSCodium

This approach is legally sound because:
- We use only open-source components
- Remove proprietary elements via VSCodium
- Create substantial original functionality
- Focus on specialized radar analysis
- Not competing with VS Code as an IDE

## APPLICATION STRUCTURE STRATEGY

### Core Architecture
We maintain VSCode's proven file structure for several reasons:
- Proven architecture for large applications
- Well-documented organization
- Efficient dependency management
- Established patterns for:
  * Window management
  * UI components
  * Service integration
  * Extension points for our custom features

### Key Structure
```bash
src/vs/                          # Core namespace maintained
├── base/                        # Base utilities and services
├── platform/                    # Platform-specific code
├── workbench/                   # Main UI components
│   ├── browser/                 # Browser-based UI
│   ├── common/                  # Shared utilities
│   ├── contrib/                 # Where we add radar features
│   └── services/               # Core services
└── code/                       # Main process code
```

Benefits of This Approach:
1. Easier to maintain VSCodium build compatibility
2. Clear locations for our modifications
3. Familiar structure for future developers
4. Proven scalability

## PHASE 1 IMPLEMENTATION: CORE SAR ANALYSIS FEATURES

### Overview
Phase 1 focuses on implementing and enhancing the core SAR data analysis capabilities demonstrated in gee4book, providing a robust foundation for future AI integration.

### Core Deliverables

1. SAR Data Management:
```typescript
// Data Loading Capabilities
- Load Sentinel-1 SAR data from GEE
- Filter by date ranges
- Handle both ascending/descending passes
- Support for multiple polarizations (VV, VH)
- Batch processing capabilities

// Visualization Parameters
- Composite filter adjustments
- Gamma correction
- Layer opacity control
- Color mapping options
```

2. AOI (Area of Interest) System:
```typescript
// AOI Management
- Draw custom AOIs on map
- Save AOIs with metadata
  * Name
  * Description
  * Area calculation
  * Creation date
  * Last modified
- View saved AOIs
- Delete AOIs
- Export AOI definitions

// AOI Data Structure
interface AOI {
    id: string;
    name: string;
    description: string;
    coordinates: GeoJSON;
    areaKm2: number;
    created: Date;
    modified: Date;
    metadata?: Record<string, any>;
}
```

3. Data Visualization:
```typescript
// View Controls
- Base map selection
- Layer management
  * Toggle visibility
  * Adjust opacity
  * Layer ordering
- Zoom/pan controls
- Scale bar
- Coordinates display

// SAR Visualization
- RGB composite display
- Single band viewing
- Split-screen comparison
  * Ascending vs Descending
  * Different dates
  * Different polarizations
```

4. Processing Parameters:
```typescript
// Adjustable Parameters
- Speckle filter settings
- Terrain correction options
- Backscatter coefficient calculation
- Composite creation options
  * Band combinations
  * Temporal compositing
  * Spatial aggregation

// Parameter Presets
- Save custom parameter combinations
- Load preset configurations
- Default settings for common use cases
```

5. Export Capabilities:
```typescript
// Export Options
- GeoTIFF export
  * Single-band
  * Multi-band composite
  * Time series
- Export resolution control
- Coordinate system selection
- Batch export functionality
- Export status tracking

// Export Metadata
- Processing parameters
- Data source information
- Time range
- AOI details
```

### Implementation Details

1. User Interface Components:
```typescript
// Main Interface Elements
├── MapView/
│   ├── BaseMap
│   ├── AOIDrawingTools
│   └── LayerControls
├── Sidebar/
│   ├── AOIManager
│   ├── ProcessingParams
│   └── ExportTools
└── StatusBar/
    ├── Coordinates
    ├── Scale
    └── ProcessingStatus
```

2. Data Processing Pipeline:
```typescript
// Processing Flow
1. AOI Selection/Creation
2. Date Range Selection
3. Data Loading and Filtering
4. Processing Parameter Application
5. Visualization/Analysis
6. Export
```

3. File Management:
```typescript
// Data Organization
project/
├── aois/
│   ├── definitions/      # AOI GeoJSON files
│   └── metadata/        # AOI metadata
├── exports/
│   ├── geotiff/        # Exported images
│   └── parameters/     # Export configurations
└── settings/
    └── presets/        # Saved parameter sets
```

### Enhanced Features from Python Implementation

1. Advanced Data Analysis:
```typescript
// Enhanced Data Query
- Track image availability
  * Count ascending/descending passes
  * Show coverage statistics
  * Display temporal density

// Time Series Analysis
- Multi-temporal compositing
- Date range statistics
- Image count tracking
  * Per pass type (ascending/descending)
  * Per polarization
  * Per time period

// Export Estimation
- File size prediction
- Processing time estimation
- Resource usage calculation
```

2. Phase 2 Potential Features:
```typescript
// Advanced Processing
1. Advanced Filter Options:
   - Focal median customization
   - Custom filter kernels
   - Speckle reduction options

2. Interactive Processing:
   - Live parameter adjustment
   - Real-time preview
   - Processing history

3. Batch Processing:
   - Multiple AOIs
   - Time series automation
   - Export queue management
```

### Technical Requirements

1. Performance Targets:
```plaintext
- AOI Drawing: Real-time response
- Data Loading: < 5s for typical AOI
- Visualization Update: < 1s
- Export Initiation: < 2s
- Maximum AOI Size: 10,000 km²
```

2. Data Handling:
```typescript
- Support for large AOIs
- Efficient memory management
- Cached visualization data
- Progressive loading for time series
```

3. Error Handling:
```typescript
- GEE connection monitoring
- Export progress tracking
- Data validation
- User input validation
- Graceful failure recovery
```

## BUILD AND DEVELOPMENT ENVIRONMENT

### Primary Development Environment
- VSCode with WSL2 Ubuntu
- Node.js v18+
- Python 3.10+
- Git for version control

### Required System Packages
```bash
# WSL2 Ubuntu dependencies
sudo apt update && sudo apt install -y \
    build-essential \
    libx11-dev \
    libxkbfile-dev \
    libsecret-1-dev \
    pkg-config \
    libglib2.0-dev \
    libgtk-3-dev \
    libnss3-dev \
    libasound2-dev
```

### Build Process
1. Initial VSCodium Build:
```bash
# In seekercode directory
./prepare_vscode.sh
yarn
yarn build
```

2. Development Testing:
```bash
# Terminal 1: Watch mode
yarn watch

# Terminal 2: Development instance
yarn run compile
./scripts/code.sh

# Debug Tools Available:
- Developer Tools (Ctrl+Shift+I)
- VS Code Debug Console
- Hot reload (Ctrl+R)
```

## PROJECT STRUCTURE

### Root Structure
```bash
~/projects/
├── seekercode/              # VSCode fork (UI framework)
│   ├── src/                 # Source code to modify
│   │   ├── vs/workbench/   # Core UI components
│   │   └── radar/          # Custom features
│   └── build/              # Build scripts
└── seekerapp/              # Main application
    ├── frontend/           # Modified UI framework
    │   ├── components/     # Custom UI components
    │   └── electron/       # Desktop integration
    └── backend/            # Python/Flask server
        ├── services/       # GEE integration
        ├── api/            # REST endpoints
        └── utils/          # Helper functions
```

## FEATURE STRATEGY

### Features to Maintain
1. Terminal Integration:
   - Essential for AI model interaction
   - Command-line tool integration
   - Data processing capabilities
   - AI training/scanning integration

2. File Exploration & Search:
   - AOI data management
   - GeoTIFF file handling
   - Dataset organization
   - Document management
   - Project workspace support

3. Core UI Components:
   - Window Management
   - Activity Bar
   - Side Panel System
   - Status Bar
   - Theming Engine

### Features to Modify
1. Extension System (simplified)
2. Settings Management (focused)
3. Workspace Handling (specialized)
4. Command Palette (customized)

## TESTING AND VERIFICATION

### Development Testing
1. Component Testing:
```bash
# Test UI components
yarn test

# Test backend
python -m pytest
```

2. Integration Testing:
- Map component integration
- GEE data processing
- UI responsiveness
- Export functionality
- Terminal integration
- File system operations

### Performance Testing
- Large dataset handling
- Memory management
- UI responsiveness
- Export operations
- Terminal performance
- File system operations

## DEPLOYMENT

### Build Outputs
```bash
dist/
├── electron/           # Desktop application
└── backend/           # Python package
```

### Distribution
1. Electron app packaging
2. Python backend deployment
3. Environment setup scripts
4. Documentation generation

## MAINTENANCE

### Version Control
- Branch naming: feature/, fix/, release/
- Commit messages: type(scope): description
- Version tagging: v0.1.0

### Documentation
- Keep README updated
- Document component modifications
- Track dependencies
- Update build instructions
- Document AI integration patterns
- Maintain file management guides

## NOTE ON VSCODE INTEGRATION
This project uses VSCode's core UI components, modified through VSCodium's build process. The resulting framework maintains essential IDE features for data management and terminal operations while adding specialized radar analysis capabilities.

## SECURITY CONSIDERATIONS
- GEE API key management
- Local file system access
- IPC message validation
- Input sanitization
- Error handling
- Terminal security
- File system permissions

This documentation is maintained by the project team and should be updated as the development progresses.