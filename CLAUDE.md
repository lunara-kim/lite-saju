# CLAUDE.md - AI Assistant Guide for lite-saju

## Project Overview

**lite-saju** (만세력 계산기) is a Korean Saju/Four Pillars of Destiny calculator web application. It calculates traditional Korean/Chinese astrology based on birth date and time.

### What is Saju (사주)?
Saju (Four Pillars of Destiny) is a form of East Asian fortune-telling based on:
- **Year Pillar (년주)** - Determines generation/social characteristics
- **Month Pillar (월주)** - Determines family/environment characteristics
- **Day Pillar (일주)** - Determines personal identity (most important)
- **Hour Pillar (시주)** - Determines later life/children characteristics

Each pillar consists of:
- **Heavenly Stem (천간)** - One of 10 celestial stems (甲乙丙丁戊己庚辛壬癸)
- **Earthly Branch (지지)** - One of 12 terrestrial branches (子丑寅卯辰巳午未申酉戌亥)

## Codebase Structure

```
lite-saju/
├── index.html      # Single-file application (HTML + CSS + JavaScript)
├── LICENSE         # MIT License
└── CLAUDE.md       # This file
```

This is a **single-file static web application** with no build process, dependencies, or external frameworks.

## Code Architecture (index.html)

The application is organized into 5 logical sections within the `<script>` tag:

### 1. Data Definitions (Lines 254-306)
```javascript
WUXING          // Five Elements (Wood, Fire, Earth, Metal, Water)
TEN_GODS        // Ten Gods relationships (십성)
UNSEONG_NAMES   // Twelve life stages (십이운성)
SKY_DATA        // 10 Heavenly Stems with properties
GROUND_DATA     // 12 Earthly Branches with properties
SOLAR_TERMS     // Solar term dates for month calculation
SINSAL_NAMES    // 12 divine generals/spirits (신살)
```

### 2. Core Calculation Logic (Lines 309-369)
- `mod(n, m)` - Safe modulo for negative numbers
- `calculateSaju()` - Main calculation function that computes all four pillars

**Calculation method:**
- Year pillar: Based on solar year (after 입춘/Ipchun adjustment)
- Month pillar: Based on solar terms (절기)
- Day pillar: Calculated from reference date (2024-01-01 = 갑자)
- Hour pillar: Based on 2-hour periods (시진)

### 3. Ten Gods & Twelve Stages (Lines 372-386)
- `getTenGod()` - Calculates relationship between stems based on Five Elements cycle
- `get12Unseong()` - Calculates life stage based on Day Master

### 4. Divine Generals (Sinsal) Logic (Lines 389-445)
- `get12Sinsal()` - Calculates 12 divine generals based on Day Branch's triple harmony group
- `getNoble()` - Calculates auspicious stars (천을귀인, 천주귀인, 문곡귀인, 태극귀인)

### 5. Rendering (Lines 450-502)
- `renderColumn()` - Renders each pillar column in the result table
- `window.onload` - Initializes with current date/time

## Key Concepts & Terminology

| Korean | Hanja | English | Description |
|--------|-------|---------|-------------|
| 천간 | 天干 | Heavenly Stems | 10 stems (甲乙丙丁戊己庚辛壬癸) |
| 지지 | 地支 | Earthly Branches | 12 branches (子丑寅卯辰巳午未申酉戌亥) |
| 오행 | 五行 | Five Elements | 목(Wood), 화(Fire), 토(Earth), 금(Metal), 수(Water) |
| 십성 | 十星 | Ten Gods | Relationships between stems |
| 일주 | 日主 | Day Master | Core identity pillar |
| 십이운성 | 十二運星 | Twelve Stages | Life cycle stages |
| 신살 | 神殺 | Divine Generals | Fortune/misfortune indicators |
| 귀인 | 貴人 | Noble Stars | Auspicious helper stars |

## Five Elements Color Mapping

```css
Wood (목)  → Green  (#10b981)  .bg-wood
Fire (화)  → Red    (#ef4444)  .bg-fire
Earth (토) → Brown  (#d97706)  .bg-earth
Metal (금) → Gray   (#94a3b8)  .bg-metal
Water (수) → Black  (#1e293b)  .bg-water
```

## Development Guidelines

### Making Changes

1. **All code is in `index.html`** - There are no separate files for CSS or JavaScript
2. **No build process** - Changes take effect immediately upon file save
3. **No dependencies** - Pure vanilla HTML/CSS/JavaScript

### Testing

- Open `index.html` directly in a browser
- Test with various birth dates including edge cases:
  - Dates before/after 입춘 (Feb 4-5)
  - Dates near solar term boundaries
  - Late night hours (23:00-00:30 spans two days in traditional reckoning)

### Code Style Conventions

- Use Korean comments for Saju-specific logic explanations
- Keep data definitions at the top of the script section
- Group related functions together with section comments
- Use descriptive variable names that reflect traditional terminology

### CSS Structure

- CSS custom properties (CSS variables) for theming at `:root`
- Mobile-responsive design with `@media (max-width: 600px)`
- BEM-like class naming (`.char-box`, `.ten-god-text`, `.badge-sinsal`)

## Important Implementation Details

### Reference Date
The day pillar calculation uses **2024-01-01** as the reference date (갑자일). All calculations are relative to this anchor.

### Solar Terms (절기)
Month boundaries follow traditional solar terms, not calendar months:
- 입춘 (Feb ~4): Start of year
- Each month starts around the 4th-8th of the calendar month

### Time Reckoning
Traditional 12 two-hour periods (시진):
- 子時 (자시): 23:30 - 01:30
- Note: Late night crosses into next day's calculation

## Common Modification Scenarios

### Adding New Noble Stars
1. Add mapping in the `getNoble()` function
2. Follow existing pattern: `const newStarMap = { daySkyIdx: [validGroundIndices] }`

### Modifying Colors/Styling
1. Update CSS variables in `:root` for global changes
2. Update `.bg-*` classes for element colors

### Adding New Sinsal Types
1. Add to `SINSAL_NAMES` array
2. Modify `get12Sinsal()` calculation logic if needed

## Language

- UI is in **Korean**
- Code comments are primarily in **Korean** with some English
- Variable names use **English** with Korean context in comments

## License

MIT License - See LICENSE file for details.
