# Tags Field Type

*anomaly.field_type.tags*

#### A tags input field type.

The tags field type provides a tag input interface for creating and managing comma-separated tags.

## Features

- Interactive tag input with autocomplete
- Free-form tag creation
- Tag filtering and validation
- Stored as JSON array in database
- Predefined tag suggestions
- Case-insensitive tag matching
- Duplicate prevention
- Easy tag removal interface

## Configuration

### Basic Configuration

```php
protected $fields = [
    'tags' => [
        'type' => 'anomaly.field_type.tags'
    ]
];
```

### With Suggested Tags

```php
'tags' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'options' => [
            'Laravel',
            'PyroCMS',
            'PHP',
            'JavaScript'
        ]
    ]
]
```

### With Filter Pattern

```php
'tags' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'filter' => '/^[a-z0-9\-]+$/i' // Only alphanumeric and hyphens
    ]
]
```

### With Max Tags

```php
'tags' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'max' => 5
    ]
]
```

## Usage Examples

### Basic Tags

```php
$stream->create([
    'tags' => ['laravel', 'php', 'web-development']
]);
```

### With Validation

```php
protected $fields = [
    'keywords' => [
        'type'  => 'anomaly.field_type.tags',
        'rules' => [
            'required'
        ]
    ]
];
```

## Accessing Values

### In Twig Templates

```twig
{# Display all tags #}
{% for tag in entry.tags %}
    <span class="badge">{{ tag }}</span>
{% endfor %}

{# Display as comma-separated list #}
{{ entry.tags|join(', ') }}

{# Check if specific tag exists #}
{% if 'laravel' in entry.tags %}
    <p>Tagged with Laravel</p>
{% endif %}

{# Count tags #}
<p>{{ entry.tags|length }} tags</p>

{# Create tag links #}
{% for tag in entry.tags %}
    <a href="/search?tag={{ tag|url_encode }}">{{ tag }}</a>
{% endfor %}
```

### In PHP

```php
$entry = $model->find(1);

// Get tags array
$tags = $entry->tags;

// Check if tag exists
if (in_array('laravel', $entry->tags)) {
    // Handle tag
}

// Count tags
$count = count($entry->tags);

// Search by tag
$entries = Model::whereJsonContains('tags', 'laravel')->get();
```

## Setting Values

### In Forms

```php
$form = $builder->make('example.module.test');
$form->on('saving', function(FormBuilder $builder) {
    $entry = $builder->getFormEntry();
    $entry->tags = ['new', 'tags', 'here'];
});
```

### Direct Assignment

```php
$entry->tags = ['laravel', 'php', 'web'];
$entry->save();

// Add a tag
$tags = $entry->tags;
$tags[] = 'new-tag';
$entry->tags = $tags;
$entry->save();

// Remove a tag
$entry->tags = array_diff($entry->tags, ['unwanted-tag']);
$entry->save();
```

## Database Structure

The tags field type stores tags as:
- **TEXT** - JSON array of tag strings

## Validation

### Required Tags

```php
'tags' => [
    'type'  => 'anomaly.field_type.tags',
    'rules' => [
        'required'
    ]
]
```

### Minimum Tags Required

```php
'tags' => [
    'type'  => 'anomaly.field_type.tags',
    'rules' => [
        'required',
        'min:3' // At least 3 tags
    ]
]
```

### Maximum Tags Limit

```php
'tags' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'max' => 5
    ],
    'rules' => [
        'max:5'
    ]
]
```

### Pattern Validation

```php
'tags' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'filter' => '/^[a-z0-9\-]+$/i'
    ]
]
```

## Common Use Cases

### Blog Post Tags

```php
'tags' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'options' => [
            'technology',
            'business',
            'tutorial',
            'news',
            'opinion'
        ]
    ]
]
```

### Product Keywords

```php
'keywords' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'max' => 10
    ],
    'rules' => [
        'required',
        'min:3'
    ]
]
```

### Category Tags

```php
'categories' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'options' => [
            'web-development',
            'mobile-development',
            'design',
            'marketing'
        ]
    ]
]
```

### SEO Meta Keywords

```php
'meta_keywords' => [
    'type'   => 'anomaly.field_type.tags',
    'config' => [
        'max'    => 5,
        'filter' => '/^[a-z0-9\s\-]+$/i'
    ]
]
```

## Best Practices

1. **Normalize Tags**: Convert to lowercase and trim whitespace
2. **Limit Count**: Set reasonable max limits (typically 5-10 tags)
3. **Provide Suggestions**: Offer predefined options for consistency
4. **Filter Input**: Use regex patterns to enforce tag format
5. **Search Optimization**: Index tags column for faster queries
6. **Tag Management**: Implement tag cleanup for unused/orphaned tags
7. **Display Consistently**: Use badges or chips for visual consistency

## Requirements

- Streams Platform ^1.10
- PyroCMS 3.10+

## License

The Tags Field Type is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

## Authors

PyroCMS, Inc. - [https://pyrocms.com](https://pyrocms.com)
Ryan Thompson - [ryan@pyrocms.com](mailto:ryan@pyrocms.com)
