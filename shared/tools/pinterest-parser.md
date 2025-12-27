# Pinterest Recipe Parser

Tool for extracting recipe data from Pinterest links.

## Purpose

Allows Basil (Sous Chef agent) to parse Pinterest recipe pins and present them cleanly.

## Usage

When a Pinterest URL is provided, attempt to:
1. Fetch the page content
2. Look for structured recipe data (JSON-LD schema)
3. Extract: title, ingredients, instructions, prep time, cook time, servings
4. Return structured data for Basil to present

## Implementation Notes

### V1 Approach (Simple)

For initial implementation, use web fetch to:
1. Access the Pinterest pin URL
2. Follow redirects to the source recipe site
3. Parse the recipe from the destination

Most recipe sites use schema.org Recipe markup, which can be extracted from:
- JSON-LD in `<script type="application/ld+json">`
- Microdata attributes
- Meta tags

### Fallback

If automatic parsing fails:
1. Basil asks user to paste the recipe text
2. Basil parses the pasted content manually
3. Still presents in structured format

### Future Enhancement

Could integrate with:
- Apify recipe scraper
- Dedicated recipe API (Spoonacular, Edamam)
- Custom scraper for common recipe sites

## Data Structure

```json
{
  "title": "Recipe Name",
  "sourceUrl": "https://original-recipe-site.com/recipe",
  "prepTime": "10 minutes",
  "cookTime": "25 minutes",
  "totalTime": "35 minutes",
  "servings": 4,
  "ingredients": [
    "1 lb ground chicken",
    "4 cloves garlic, minced",
    "..."
  ],
  "instructions": [
    "Step 1...",
    "Step 2...",
    "..."
  ],
  "notes": "Optional recipe notes"
}
```

## Gluten Flagging

After parsing, Basil should scan ingredients for gluten-containing items:
- Soy sauce
- Oyster sauce
- Fish sauce (some brands)
- Flour
- Breadcrumbs
- Pasta
- Anything with wheat, barley, rye, malt

Flag these and suggest GF alternatives.
