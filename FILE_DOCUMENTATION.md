# 📋 Social Mood Matcher - Complete File Documentation

*Generated: March 3, 2026*

---

## 📑 Table of Contents
1. [Root Level Files](#root-level-files)
2. [Configuration Module](#configuration-module)
3. [Services Module](#services-module)
4. [Utilities Module](#utilities-module)
5. [Tests Module](#tests-module)
6. [Cache & Support Files](#cache--support-files)

---

## 🎯 Project Overview

**Social Mood Matcher** is a production-ready Streamlit application that uses AI to analyze images and generate engaging social media captions with trending hashtags. It combines computer vision (BLIP), sentiment analysis (DistilBERT), and the Google Gemini API to provide intelligent, platform-aware social media content generation.

---

## Root Level Files

### 📄 [app.py](app.py) - Main Application Entry Point
**Type:** Python Module (612 lines)  
**Purpose:** Core Streamlit web application interface

**Key Features:**
- **Page Configuration**: Streamlit app setup with custom title, layout, and sidebar
- **Custom CSS Styling**: Extensive material design-inspired UI with:
  - Gradient headers and sentiment badges
  - High-contrast caption and hashtag boxes
  - Responsive button styling with hover effects
  - Character counter with status indicators (green/yellow/red)
- **User Interface Components**:
  - Image upload section with drag-and-drop support
  - Sidebar controls for caption style, platform selection, and hashtag count
  - Real-time processing feedback and loading indicators
  - Results display with copy-to-clipboard functionality
- **Application Pipeline**:
  1. Image validation and loading
  2. Sentiment detection and classification
  3. Caption generation in multiple styles
  4. Hashtag recommendation based on sentiment and category
  5. Character limiting for platform compatibility
  6. Final output formatting and display
- **Integration Points**:
  - Imports all services (sentiment, caption, hashtag, character limiter)
  - Uses configuration settings from `config/settings.py`
  - Handles both local models and Google Gemini API
  - Streamlit caching for performance optimization

**User Flow:**
```
User Upload → Validation → Sentiment Detection → Caption Generation → 
Hashtag Engine → Character Limiter → Display Results → Copy Output
```

---

### 📋 [README.md](README.md) - Project Documentation
**Type:** Markdown Document (341 lines)  
**Purpose:** Comprehensive project guide and documentation

**Sections Included:**
1. **Overview** - High-level project description
2. **Key Features** - List of major capabilities
3. **Architecture** - Visual flowchart and project structure
4. **Installation Guide** - Step-by-step setup instructions
5. **Usage Instructions** - How to run and use the application
6. **API Configuration** - Details on setting up Google Gemini API
7. **Deployment** - Production deployment recommendations
8. **File Structure** - Complete project directory layout
9. **Dependencies** - Explanation of each package
10. **Troubleshooting** - Common issues and solutions

---

### 📦 [requirements.txt](requirements.txt) - Python Dependencies
**Type:** Plain Text Configuration (25 lines)  
**Purpose:** Lists all required Python packages and versions

**Core Dependencies:**
```
streamlit>=1.31.0              # Web framework
transformers>=4.36.0           # HuggingFace models
torch>=2.0.0                   # PyTorch framework
torchvision>=0.15.0            # Computer vision library
Pillow>=10.0.0                 # Image processing
google-generativeai>=0.3.0     # Google Gemini API
python-dotenv>=1.0.0           # Environment variable management
numpy>=1.24.0                  # Numerical computing
pytest>=7.4.0                  # Testing framework
```

**Installation:** `pip install -r requirements.txt`

---

### 🔐 [.env](app.py) - Environment Configuration
**Type:** Environment Variables (10 lines)  
**Purpose:** Stores sensitive API keys and configuration

**Key Variables:**
```
DEBUG=False                    # Debug mode toggle
LOG_LEVEL=INFO                 # Logging level
MODEL_CACHE_DIR=./models_cache # Where to store AI models
GEMINI_API_KEY=***             # Google Gemini API key
AI_MODEL=gemini                # Model selection (local or gemini)
```

**Security Note:** This file contains sensitive API keys and should never be committed to version control.

---

### 📝 [GEMINI_SETUP.md](GEMINI_SETUP.md) - Gemini API Setup Guide
**Type:** Markdown Documentation (121 lines)  
**Purpose:** Step-by-step guide for setting up Google Gemini API

**Steps Covered:**
1. Getting API key from Google AI Studio
2. Creating `.env` file
3. Installing Google Generative AI SDK
4. Restarting the application
5. Troubleshooting common issues

---

### 🚫 [.gitignore](.gitignore) - Git Ignore Rules
**Type:** Configuration File (69 lines)  
**Purpose:** Specifies files and folders to exclude from version control

**Key Exclusions:**
- `__pycache__/` - Python cache files
- `*.pyc` - Compiled Python files
- `.venv/`, `venv/` - Virtual environments
- `.env` - Environment files with secrets
- `models_cache/` - Large AI models (2GB+)
- `.vscode/`, `.idea/` - IDE configuration
- `.streamlit/` - Streamlit cache

---

## 🔧 Configuration Module (`config/`)

### 📄 [settings.py](config/settings.py) - Centralized Configuration
**Type:** Python Module (173 lines)  
**Purpose:** Single source of truth for all application configuration

**Configuration Sections:**

#### 1. **Model Configuration**
```python
MODEL_CONFIG = {
    "image_captioning": {
        "model_name": "Salesforce/blip-image-captioning-base",
        "cache_dir": "./models_cache",
        "device": "cpu"  # or "cuda" for GPU
    },
    "sentiment": {
        "model_name": "distilbert-base-uncased-finetuned-sst-2-english"
    },
    "caption_generation": {
        "model_name": "gpt2",
        "max_length": 100,
        "temperature": 0.8
    }
}
```

#### 2. **Character Limits** (Platform-Specific)
```python
CHARACTER_LIMITS = {
    "twitter": 280,          # X/Twitter limit
    "instagram": 2200,       # Instagram limit
    "facebook": 63206,       # Facebook limit
    "default": 280
}
```

#### 3. **Sentiment Categories**
Defines 10 emotional sentiments:
- happy, calm, cozy, aesthetic, adventurous
- luxury, energetic, peaceful, romantic, nostalgic

#### 4. **Caption Styles**
Four distinct writing styles with characteristics:
- **Casual** - Friendly, moderate emoji, low formality
- **Aesthetic** - Poetic, minimal emoji, medium formality
- **Professional** - Polished, no emoji, high formality
- **Playful** - Energetic, high emoji, low formality

#### 5. **Trending Hashtags 2024 Database**
Categorized hashtags for:
- **Food**: General, happy, luxury, cozy, aesthetic
- **Travel**: Adventure-focused, calm, luxury, aesthetic
- **Nature**: Peaceful, adventurous, aesthetic, energetic
- **Lifestyle**: Various sentiments
- **General**: 2024 trending tags

#### 6. **UI Configuration**
- Page title, icon, layout settings
- Max upload size (10MB)
- Supported formats (jpg, png, webp)

#### 7. **Image Processing Settings**
- Max dimension: 1024px
- Thumbnail size: 512x512
- Compression quality: 85%

#### 8. **Caching Configuration**
- Model caching enabled
- Result caching with 1-hour TTL
- Performance optimization

#### 9. **API Keys Management**
Stores references to external API keys:
- Gemini API
- OpenAI API
- HuggingFace API
- Twitter API

---

## 🛠️ Services Module (`services/`)

### 🎨 [image_sentiment.py](services/image_sentiment.py) - Image Analysis Service
**Type:** Python Module (215 lines)  
**Purpose:** Analyzes images to detect mood/sentiment

**Main Class:** `ImageSentimentDetector`

**Key Methods:**

#### `__init__()`
- Loads BLIP model for image captioning
- Initializes DistilBERT sentiment classifier
- Sets up sentiment keyword mappings

#### `generate_caption(image) → str`
- Uses BLIP to generate descriptive image captions
- Processes image through vision-language model
- Returns natural language description

#### `analyze_sentiment_from_text(text) → Dict[str, float]`
- Analyzes text descriptions to identify sentiment
- Uses keyword matching against sentiment vocabularies
- Combines base sentiment + keyword matching
- Returns confidence scores for each sentiment category

**Sentiment Mapping:**
Each sentiment has associated keywords that boost its score:
- **Happy**: bright, vibrant, smiling, fun
- **Calm**: peaceful, tranquil, gentle, soft
- **Cozy**: warm, comfortable, homey, snug
- **Aesthetic**: artistic, elegant, minimalist
- **Adventurous**: wild, outdoor, hiking
- **Luxury**: premium, sophisticated, gourmet
- etc.

**Processing Pipeline:**
```
Image → BLIP Captioning → Text Description → 
Sentiment Analysis → Keyword Matching → 
Matched Scores → Final Sentiment Results
```

---

### ✍️ [caption_generator.py](services/caption_generator.py) - Caption Service
**Type:** Python Module (371 lines)  
**Purpose:** Generates engaging social media captions

**Main Class:** `CaptionGenerator`

**Features:**
- 10 sentiment categories × 4 caption styles = **40+ caption templates**
- Each template includes multiple variations

**Caption Structure:**
```python
caption_templates = {
    "happy": {
        "casual": [...],      # Casual style quotes
        "aesthetic": [...],   # Poetic variations
        "professional": [...],# Business-appropriate
        "playful": [...]      # Fun and energetic
    },
    # ... other sentiments
}
```

**Key Methods:**

#### `generate_caption(sentiment, style) → str`
- Selects appropriate template based on sentiment + style
- Randomly chooses variation for uniqueness
- Combines user input with template elements

#### `generate_caption_with_context(sentiment, style, context) → str`
- Incorporates image context into caption
- Uses context words/phrases in generation
- Creates more personalized captions

#### `generate_caption_variants(caption, style) → Dict[str, str]`
- Creates multiple versions of single caption
- Suitable for A/B testing on social media
- Different lengths and tones

**Template Examples (Happy + Casual):**
- "Feeling absolutely amazing! ☀️"
- "This just made my day! 😊"
- "Can't stop smiling! 💛"
- "Living my best life! ✨"

---

### #️⃣ [hashtag_engine.py](services/hashtag_engine.py) - Hashtag Service
**Type:** Python Module (233 lines)  
**Purpose:** Generates trending, relevant hashtags

**Main Class:** `HashtagEngine`

**Features:**
- Access to 2024 trending hashtags database
- Sentiment-aware hashtag selection
- Category-specific recommendations
- Fallback hashtags for reliability

**Key Methods:**

#### `get_hashtags(category, sentiment, count) → List[str]`
- Retrieves hashtags based on image category (food/travel/nature/lifestyle)
- Prioritizes sentiment-specific tags first
- Fills remaining slots with general category tags
- Uses fallback tags as last resort

**Selection Algorithm:**
```
1. Get sentiment-specific hashtags (40%)
2. Add general category hashtags (40%)
3. Add general 2024 trending tags (20%)
4. Ensure unique hashtags
5. Return requested quantity
```

#### `get_hashtags_by_priority(category, sentiment, all_sentiments) → List[str]`
- Uses all sentiment confidence scores
- Primary sentiment gets 40% of hashtags
- Secondary sentiment gets 20%
- Remaining 40% from category/trending

**Hashtag Database Structure:**
```python
TRENDING_HASHTAGS_2024 = {
    "food": {
        "general": ["#Foodie", "#FoodPhotography", ...],
        "happy": ["#FoodJoy", "#HappyEating", ...],
        "luxury": ["#FineDining", "#GourmetFood", ...],
        ...
    },
    "travel": {...},
    "nature": {...},
    "lifestyle": {...},
    "general": {"2024_trending": [...]}
}
```

---

### 📏 [character_limiter.py](services/character_limiter.py) - Character Service
**Type:** Python Module (243 lines)  
**Purpose:** Ensures content fits platform character limits

**Main Class:** `CharacterLimiter`

**Key Methods:**

#### `get_limit(platform) → int`
- Returns character limit for platform
- Supports: Twitter (280), Instagram (2200), Facebook (63206)

#### `check_limit(text, platform) → Tuple[bool, int, int]`
- Validates if text fits within platform limit
- Returns: (fits_limit, character_count, limit)

#### `limit_text(caption, hashtags, platform) → Tuple[str, str, Dict]`
- **Smart truncation** prioritizing caption over hashtags
- Preserves word boundaries
- Returns metadata about truncation
- Intelligently removes hashtags if needed before shortening caption

**Truncation Strategy:**
```
1. Check if combined text fits
2. If not: Remove hashtags one-by-one
3. Then: Smartly truncate caption at word boundary
4. Preserve sentence meaning
5. Return truncated versions + metadata
```

#### `format_for_platform(caption, hashtags, platform) → str`
- Formats final output for specific platform
- Adds platform-appropriate separators
- Returns "ready to copy-paste" format

---

### 🔮 [gemini_service.py](services/gemini_service.py) - Gemini API Service
**Type:** Python Module (274 lines)  
**Purpose:** Alternative image analysis using Google Gemini API

**Main Class:** `GeminiVisionAnalyzer`

**Features:**
- Advanced vision-language model (Gemini 1.5 Flash)
- More sophisticated understanding than local models
- Multi-modal analysis capabilities
- Graceful fallback to local models

**Key Methods:**

#### `__init__(api_key) → None`
- Initializes Google Generative AI client
- Validates API key from `.env` or environment
- Loads Gemini 1.5 Flash model

#### `analyze_image_sentiment(image) → Dict`
- Uses Gemini vision to analyze image mood
- Generates description, detects sentiment, calculates confidence
- Categorizes image (food/travel/nature/lifestyle)
- Returns structured analysis result

**Gemini Prompt:**
```
Analyze image and provide:
1. Description (1-2 sentences)
2. Primary mood from predefined list
3. Confidence score (0-1)
4. Image category
```

#### `generate_caption_variants(image, sentiment, category) → Dict[str, str]`
- Generates 3 distinct caption variants:
  - **Punchy**: Short, high-impact (< 10 words)
  - **Aesthetic**: Poetic, "Instagrammable"
  - **Engagement**: Interactive, question-based

#### `_parse_gemini_response(response_text) → Dict`
- Parses structured Gemini output
- Error handling and fallback values
- Normalized confidence scores

**Error Handling:**
- Catches API failures gracefully
- Falls back to safe default values
- Returns success flag for error detection

---

## 🔨 Utilities Module (`utils/`)

### 🖼️ [image_utils.py](utils/image_utils.py) - Image Processing Utilities
**Type:** Python Module (182 lines)  
**Purpose:** Image validation, loading, and preprocessing

**Main Class:** `ImageProcessor`

**Key Methods:**

#### `validate_image(uploaded_file) → Tuple[bool, Optional[str]]`
- Checks file size (max 10MB)
- Validates file extension (jpg, png, webp)
- Returns validation status and error message

#### `load_image(uploaded_file) → Optional[Image.Image]`
- Reads Streamlit uploaded file
- Converts to PIL Image
- Handles RGBA → RGB conversion
- Graceful error handling

#### `resize_image(image, max_dimension) → Image.Image`
- Resizes while maintaining aspect ratio
- Uses LANCZOS resampling (high quality)
- Prevents oversizing of small images

**Resize Algorithm:**
```
If image ≤ max_dimension: return as-is
Else:
  Calculate aspect ratio
  Resize to max_dimension on longest side
  Maintain aspect ratio on other side
```

#### `create_thumbnail(image) → Image.Image`
- Creates square thumbnail (512×512)
- Used for UI display
- Memory efficient

#### `preprocess_for_model(image) → Image.Image`
- Prepares image for model input
- Resizes, normalizes, converts format
- Model-ready output

**Supported Formats:**
- JPEG/JPG
- PNG
- WebP

---

### 📝 [text_utils.py](utils/text_utils.py) - Text Processing Utilities
**Type:** Python Module (253 lines)  
**Purpose:** Text cleaning, formatting, and manipulation

**Main Class:** `TextProcessor`

**Key Methods:**

#### `clean_text(text) → str`
- Removes extra whitespace
- Removes special characters (keeps emoji)
- Capitalizes first letter
- Returns sanitized text

#### `format_hashtags(hashtags) → str`
- Ensures # prefix on all hashtags
- Joins into single string with spaces
- Handles already-formatted hashtags

#### `extract_hashtags(text) → List[str]`
- Finds all hashtags in text
- Uses regex pattern: `#\w+`
- Returns list of hashtags

#### `remove_hashtags(text) → str`
- Strips all hashtags from text
- Cleans up extra spaces
- Returns caption-only text

#### `truncate_text(text, max_length, preserve_words) → str`
- Simple truncation to max length
- Optional word-boundary preservation
- Removes trailing spaces

#### `smart_truncate_with_hashtags(caption, hashtags, max_length) → Tuple[str, str]`
**Smart Algorithm:**
```
1. Check if combined text fits
2. If yes: return both as-is
3. If no: try removing hashtags first
4. If still over: shorten caption progressively
5. Remove words while maintaining meaning
6. Return shortened caption + remaining hashtags
```

**Priority:** Caption content > Hashtags

#### `add_line_breaks(text, width) → str`
- Adds line breaks for readability
- Prevents text wrapping issues
- Useful for long captions

#### `count_engagement_elements(caption) → Dict`
- Counts emojis, symbols, punctuation
- Returns engagement metrics
- Predicts engagement potential

---

### 🔗 [__init__.py Files]
**Location:** `config/__init__.py`, `services/__init__.py`, `utils/__init__.py`  
**Purpose:** Makes directories Python packages  
**Content:** Optional imports for convenience

---

## 🧪 Tests Module (`tests/`)

### ✅ [test_pipeline.py](tests/test_pipeline.py) - Test Suite
**Type:** Python Module (333 lines)  
**Purpose:** Comprehensive testing of all components

**Testing Framework:** pytest

**Test Classes:**

#### `TestImageProcessor`
Tests image processing utilities:
- Image creation and validation
- Resize operations
- Format conversions
- Preprocessing for models

#### `TestTextProcessor`
Tests text processing utilities:
- Text cleaning
- Hashtag formatting and extraction
- Text truncation
- Smart truncation with hashtags

#### `TestCaptionGenerator`
Tests caption generation:
- Single caption generation
- Multiple style generation
- Context-aware generation
- Variation generation

#### `TestHashtagEngine`
Tests hashtag recommendations:
- Category-based hashtag selection
- Sentiment-based hashtag selection
- Hashtag count validation
- Fallback hashtag handling

#### `TestCharacterLimiter`
Tests character limiting:
- Platform limit checking
- Smart text limiting
- Hashtag preservation
- Metadata generation

#### `TestIntegration`
End-to-end pipeline tests:
- Full image-to-output pipeline
- Multi-platform support
- Error handling
- Performance benchmarks

**Running Tests:**
```bash
pytest tests/test_pipeline.py -v
pytest tests/test_pipeline.py::TestImageProcessor -v
pytest tests/test_pipeline.py -v --cov=services
```

---

## 📦 Cache & Support Files

### `models_cache/` Directory
**Type:** Model Cache  
**Purpose:** Stores downloaded AI models locally

**Size:** ~2GB (one-time download)

**Cached Models:**
1. **BLIP Image Captioning**
   - `Salesforce/blip-image-captioning-base`
   - Files: model.safetensors, config.json
   
2. **DistilBERT Sentiment Classifier**
   - `distilbert-base-uncased-finetuned-sst-2-english`
   - Files: config.json, tokenizer files

3. **GPT-2 (for local caption generation)**
   - Model weights and configuration

**Cache Structure:**
```
models_cache/
├── models--Salesforce--blip-image-captioning-base/
│   ├── refs/
│   └── snapshots/
└── other_model_caches/
```

### `__pycache__/` Directories
**Type:** Python Cache  
**Purpose:** Compiled Python bytecode  
**Generated:** Automatically  
**Action:** Excluded from Git

---

## 🔄 Application Data Flow

```
┌─────────────────────────────────────────────────────────┐
│                  USER UPLOADS IMAGE                      │
└────────────────────┬────────────────────────────────────┘
                     │
              ┌──────▼──────┐
              │  Validation │ ← ImageProcessor.validate_image()
              └──────┬──────┘
                     │ (If valid)
        ┌────────────▼──────────────┐
        │  Load Image to PIL Format │ ← ImageProcessor.load_image()
        └────────────┬──────────────┘
                     │
        ┌────────────▼────────────────────┐
        │ Image Sentiment Detection       │ ← ImageSentimentDetector
        │ (BLIP + DistilBERT)             │
        └────────────┬───────────────────┘
                     │
    ┌────────────────┼────────────────────┐
    │                │                    │
┌───▼──────┐  ┌─────▼─────┐  ┌──────▼───┐
│ Sentiment│  │ Confidence│  │ Category │
│ Category │  │   Score   │  │ Detection│
└───┬──────┘  └─────┬─────┘  └──────┬───┘
    │                │              │
    └────────────────┼──────────────┘
                     │
         ┌───────────▼──────────┐
         │ Caption Generation   │ ← CaptionGenerator
         │ (4 Styles Available) │
         └───────────┬──────────┘
                     │
         ┌───────────▼──────────┐
         │ Hashtag Generation   │ ← HashtagEngine
         │ (Sentiment-Aware)    │
         └───────────┬──────────┘
                     │
         ┌───────────▼──────────────┐
         │ Character Limitation     │ ← CharacterLimiter
         │ (Platform-Specific)      │
         └───────────┬──────────────┘
                     │
        ┌────────────▼────────────┐
        │ Format for Display      │
        │ (Copy-to-Clipboard)     │
        └────────────┬────────────┘
                     │
       ┌─────────────▼──────────────┐
       │  Display Results to User   │
       │  (Streamlit Interface)     │
       └────────────────────────────┘
```

---

## 🚀 Key Technologies

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Web Framework** | Streamlit 1.31+ | Interactive UI |
| **Vision-Language** | BLIP (Salesforce) | Image captioning |
| **Sentiment Analysis** | DistilBERT | Emotion detection |
| **Advanced Vision** | Google Gemini 1.5 | Enhanced analysis |
| **Caption Generation** | GPT-2/Gemini | Text generation |
| **Image Processing** | PIL/Pillow | Image handling |
| **ML Framework** | PyTorch 2.0+ | Deep learning |
| **Testing** | Pytest | Quality assurance |

---

## 📊 Configuration Hierarchy

```
.env (Secrets)
    ↓
config/settings.py (Central Config)
    ├── MODEL_CONFIG
    ├── CHARACTER_LIMITS
    ├── SENTIMENT_CATEGORIES
    ├── CAPTION_STYLES
    ├── TRENDING_HASHTAGS_2024
    └── UI_CONFIG
    
Used by:
├── services/ (All service classes)
├── utils/ (Utility functions)
└── app.py (Main application)
```

---

## 🔐 Data Flow & Privacy

1. **Image Upload** → Processed locally
2. **Models** → Downloaded once, cached locally
3. **Gemini API** → Optional, send image only if enabled
4. **Results** → Generated locally, no external storage
5. **User Data** → Never saved or logged
6. **API Keys** → Stored in `.env` (git-ignored)

---

## File Statistics Summary

| Module | Files | Lines | Purpose |
|--------|-------|-------|---------|
| **Root** | 4 | 1,235+ | Main app + docs |
| **Config** | 1 | 173 | Configuration |
| **Services** | 5 | 1,500+ | Core logic |
| **Utils** | 2 | 435 | Helpers |
| **Tests** | 1 | 333 | Quality assurance |
| **Total** | 13+ | 3,700+ | Complete project |

---

## 🎯 Next Steps for Users

1. **Setup**: Install dependencies and configure `.env` with Gemini API key
2. **Run**: Execute `streamlit run app.py`
3. **Test**: Use provided test images or upload your own
4. **Deploy**: Follow deployment guide in README.md
5. **Extend**: Add new caption styles or hashtag categories in config

---

*Documentation prepared for Social Mood Matcher v1.0.0*
