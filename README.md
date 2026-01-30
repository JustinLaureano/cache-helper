Cache helper class

```php

use Illuminate\Support\Facades\Cache;

class CacheHelper
{
    public static method remember(ResourceCacheContract $resource)
    {
        Cache::remember($resource->getCacheKey(), $resource->getCacheDuration(), $resource->getCacheCallback);
    }
}

```


contract to implement

```php

interface ResourceCacheContract
{
    public function getCacheKey();
    public function getCacheDuration();
    public function getCacheCallback();
}

```

resource using the contract

```php

class LocalResource extends PackageResource implements ResourceCacheContract
{
    public function getCacheKey()
    {
        return 'resource_id_'. $this->id;
    }

    public function getCacheDuration()
    {
        return 10;
    }

    public function getCacheCallback()
    {
        return ResourceService::fetchApiData();
    }
}

```

calling the cache helper

```php

$localResource = new LocalResource();

CacheHelper::remember($localResource);

```