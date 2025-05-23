# Django Sorl Async Thumbnails

Asynchronous thumbnail generation for [sorl-thumbnail](https://github.com/jazzband/sorl-thumbnail) using Celery.

## Installation

```bash
pip install django-sorl-async-thumbnails
```

## Configuration

1. Add `sorl_async_thumbnails` to `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    'sorl.thumbnail',
    'sorl_async_thumbnails',
    ...
]
```

2. Configure Celery in your project.

3. Add settings to `settings.py`:

```python
# Default settings
THUMBNAIL_BACKEND = 'sorl_async_thumbnails.backend.AsyncThumbnailBackend' #Set AsyncThumbnailBackend as backend for sorl.thumbnails
ASYNC_THUMBNAILS_BY_DEFAULT = False  # Generate thumbnails asynchronously by default
ASYNC_THUMBNAILS_PLACEHOLDER = True  # Use placeholder during generation
ASYNC_THUMBNAILS_PLACEHOLDER_STATICFILE = 'placeholder.jpg'  # Path to placeholder image
ASYNC_THUMBNAILS_GENERATE_PLACEHOLDER_THUMB = True  # Generate thumbnail for placeholder
ASYNC_THUMBNAILS_STATICFILES_STORAGE = 'django.contrib.staticfiles.storage.StaticFilesStorage'  # Storage for placeholder
```

## Usage

To generate thumbnails in synchronous mode regardless of settings, use the create_async=False keyword

### Example

```python
# In your model
from sorl.thumbnail import get_thumbnail 

class MyModel(models.Model):
    image = models.ImageField(upload_to='images/')
    
    def get_thumbnail(self):
        backend = AsyncThumbnailBackend()
        # If ASYNC_THUMBNAILS_BY_DEFAULT is True, this will generate thumbnail asynchronously
        return get_thumbnail(self.image, '100x100')
    
    def get_thumbnail_sync(self):
        backend = AsyncThumbnailBackend()
        # Force synchronous generation
        return get_thumbnail(self.image, '100x100', create_async=False)
    
    def get_thumbnail_async(self):
        backend = AsyncThumbnailBackend()
        # Force asynchronous generation
        return get_thumbnail(self.image, '100x100', create_async=True)
```

## License

MIT 