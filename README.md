Phalcon + Elastic
=================

# How to use

Create your model

```php
namespace Models;

class Project extends Injectable
{
    use ElasticModelTrait;
    
    protected static $index = 'phalconist';
    protected static $type = 'project';
    
    /**
     * @param int $limit
     * @return mixed
     */
    public static function myQuery($limit = 25)
    {
        $query = [
            'aggs' => [
                'types' => [
                    'terms' => [
                        'field' => 'composer.type',
                        'size' => $limit,
                    ],
                ]
            ]
        ];
        $resultSet = static::getStorage()->search($query);
        return static::toTags($resultSet->getAggregation('types')['buckets'], 'key', 'doc_count');
    }
}
```

Use

```
$result = Project::myQuery(25);
```

License
-------

This project is open-sourced software licensed under the MIT License. See the LICENSE file for more information.