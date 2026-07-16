# Modular Structure Implementation Guide

## Step 1: Create New Directory Structure

```bash
# Create core module directories
mkdir -p strategic_exit/core/static/{css,js,images/{logos,icons,backgrounds},fonts}
mkdir -p strategic_exit/core/templates/{base,layouts,components}

# Create business assessment module directories
mkdir -p strategic_exit/business_assessment/static/{css,js,images/{assessment,survey,valuation}}
mkdir -p strategic_exit/business_assessment/templates/{assessment,survey,valuation}

# Create dashboard module directories
mkdir -p strategic_exit/dashboard/static/{css,js,images/dashboard}
mkdir -p strategic_exit/dashboard/templates/dashboard

# Create users module directories
mkdir -p strategic_exit/users/static/{css,js,images/users}
mkdir -p strategic_exit/users/templates/users

# Create shared module directories
mkdir -p strategic_exit/shared/static/{css,js,images/shared}
mkdir -p strategic_exit/shared/templates/shared
```

## Step 2: Move Existing Files

### Core Module Files:
```bash
# Move base templates
mv strategic_exit/templates/__base.html strategic_exit/core/templates/base/
mv strategic_exit/templates/base.html strategic_exit/core/templates/base/

# Move main CSS files
mv strategic_exit/static/css/main.css strategic_exit/core/static/css/base.css
mv strategic_exit/static/css/project.css strategic_exit/core/static/css/utilities.css

# Move logo and common images
mv strategic_exit/static/images/logo.png strategic_exit/core/static/images/logos/
mv strategic_exit/static/images/logo-footer.png strategic_exit/core/static/images/logos/
mv strategic_exit/static/images/favicons/ strategic_exit/core/static/images/icons/
```

### Business Assessment Module Files:
```bash
# Move assessment templates
mv strategic_exit/business_assessment/templates/business_assessment/* strategic_exit/business_assessment/templates/assessment/

# Move assessment images
mv strategic_exit/static/images/survey-img.png strategic_exit/business_assessment/static/images/survey/
mv strategic_exit/static/images/valuation-img.png strategic_exit/business_assessment/static/images/valuation/
mv strategic_exit/static/images/pdf-*.jpeg strategic_exit/business_assessment/static/images/assessment/
```

### Dashboard Module Files:
```bash
# Move dashboard templates
mv strategic_exit/templates/landing_dashboard.html strategic_exit/dashboard/templates/dashboard/

# Move dashboard images
mv strategic_exit/static/images/dashboard-img.png strategic_exit/dashboard/static/images/dashboard/
```

## Step 3: Update Template References

### Update base template:
```html
<!-- Old -->
<link rel="stylesheet" href="{% static 'css/main.css' %}" />

<!-- New -->
<link rel="stylesheet" href="{% static 'core/css/base.css' %}" />
<link rel="stylesheet" href="{% static 'core/css/utilities.css' %}" />
```

### Update image references:
```html
<!-- Old -->
<img src="{% static 'images/logo.png' %}" />

<!-- New -->
<img src="{% static 'core/images/logos/logo.png' %}" />
```

## Step 4: Update Django Settings

### Add to settings.py:
```python
STATICFILES_DIRS = [
    BASE_DIR / "strategic_exit" / "core" / "static",
    BASE_DIR / "strategic_exit" / "business_assessment" / "static",
    BASE_DIR / "strategic_exit" / "dashboard" / "static",
    BASE_DIR / "strategic_exit" / "users" / "static",
    BASE_DIR / "strategic_exit" / "shared" / "static",
]
```

## Step 5: Create Module-Specific CSS Files

### core/static/css/components.css:
```css
/* Reusable components */
.btn-primary { /* ... */ }
.btn-outline-orange { /* ... */ }
.card-bg { /* ... */ }
```

### business_assessment/static/css/assessment.css:
```css
/* Assessment-specific styles */
.assessment-form { /* ... */ }
.survey-question { /* ... */ }
.valuation-calculator { /* ... */ }
```

### dashboard/static/css/dashboard.css:
```css
/* Dashboard-specific styles */
.dashboard-card { /* ... */ }
.chart-container { /* ... */ }
.metrics-grid { /* ... */ }
```

## Step 6: Update URL Patterns

### Update business_assessment/urls.py:
```python
# Add template directory to view context
def get_template_names(self):
    return [f"business_assessment/assessment/{self.template_name}"]
```

## Step 7: Testing Checklist

- [ ] All templates load correctly
- [ ] All static files (CSS, JS, images) load
- [ ] No broken image links
- [ ] All functionality works as before
- [ ] Mobile responsiveness maintained

## Step 8: Clean Up

```bash
# Remove old directories after testing
rm -rf strategic_exit/static/css/main.css
rm -rf strategic_exit/static/css/project.css
# Keep backup of old structure for safety
```

## Benefits After Implementation:

1. **Clear File Organization**: Each feature has its own directory
2. **Easy Asset Management**: Find images, CSS, JS quickly
3. **Better Scalability**: Add new features easily
4. **Mobile App Ready**: Clear asset paths for mobile developers
5. **Maintainable Code**: No more scattered files

## File Path Examples After Restructure:

- **Logo**: `strategic_exit/core/static/images/logos/logo.png`
- **Survey Image**: `strategic_exit/business_assessment/static/images/survey/survey-img.png`
- **Dashboard CSS**: `strategic_exit/dashboard/static/css/dashboard.css`
- **Assessment Template**: `strategic_exit/business_assessment/templates/assessment/assessment_form.html`

This structure will make your codebase much more professional and easier to maintain!

