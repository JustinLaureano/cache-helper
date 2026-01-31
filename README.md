Cache helper class

```php

use Illuminate\Support\Facades\Cache;

class CacheHelper
{
    public static method remember($resource)
    {
        $cacheContract = self::getResourceCacheContract($resource);

        Cache::remember(
            $cacheContract->getCacheKey(),
            $cacheContract->getCacheDuration(),
            $cacheContract->getCacheCallback
        );
    }

    private function getResouceCacheContract($resource)
    {
        if (! is_object($resource)) {
            throw Exception();
        }

        $resourceClass = class_basename( get_class($resource) );

        $contractClass = "App\\Contracts\\Cache\\{$resourceClass}CacheContract";

        if (! class_exists($contractClass)) {
            throw ClassNotExistsException();
        }

        $contract = new $contractClass();

        if (! $contract instanceof ResourceCacheContract) {
            throw InvalidClassException();
        }

        return $contract;
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

namespace App\Contracts\Cache;

class CommunityCacheContract implements ResourceCacheContract
{
    public function getCacheKey()
    {
        return 'community_id_'. $this->id;
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

$resource = new Community();

CacheHelper::remember($resource);

```