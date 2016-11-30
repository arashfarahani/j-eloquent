# j-eloquent
Convert eloquent date attributes to Jalali (Persian) dates on the fly, With the help of convention over configuration.
This package was forked from [reshadman/j-eloquent](http://github.com/reshadman/j-eloquent) in previous version 

#### Installation
require following line in  your composer ```require``` secion :


```
composer require arashfarahani/j-eloquent
```

```
"require" : {
    "arashfarahani/j-eloquent" : "1.0" 
}
```

#### Documentation

##### The ```PersianDateTrait``` :
By using ```\Diamond\JEloquent\PersianDateTrait``` trait in your models you can enable the plugin :
```php
use Diamond\JEloquent\PersianDateTrait;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    use PersianDateTrait;
    
    protected $table = 'users';
}
```

#### Usage
By default you can access your eloquent date attributes in jalali date by adding a ```jalali_``` substring to the begining of your original attribute like ```jalali_created_at``` :

```php
$user = Auth::user();
$user->create_at; // 2014/12/30 22:12:34
$user->jalali_created_at; // 93/09/08 22:12:34
```

##### Changing ```jalali_``` prefix
You can change the jalali date convention prefix with overriding ```$model->getJalaliPrefix()``` and or by overrriding ```$model->jalaliPrefix``` property on your model class :

```php
use Diamond\JEloquent\PersianDateTrait;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    use PersianDateTrait;

    protected $jalaliPrefix = 'persian_';
}

# or

class User extends Model {
    use PersianDateTrait;

    protected function getJalaliPrefix()
    {
        // return any format you want
        return 'persian_';
    }
}
```

##### Change jalali format
You can change the jalali format ```$model->setJalaliFormat($format)``` and ```$model->getJalaliFormat();``` or by overrriding ```$model->jalaliDateFormat``` property on your model class :

```php
use Diamond\JEloquent\PersianDateTrait;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    use PersianDateTrait;

    protected $jalaliDateFormat = 'l j F Y H:i';
}

# or

class User extends Model {
    use PersianDateTrait;

    public function setJalaliFormat($format){
        // do custom things here
        $this->jalaliDateFormat = $format; 
        
        return $this;
    }

    protected function getJalaliFormat()
    {
        // return any format you want
        return 'l j F Y H:i';
    }
}
```

##### Custom date attributes :
You can tell Eloquent that which one of your fields are date attributes like created_at and updated_at, then Eloquent treats them like ```Carbon``` objects you define multiple date attributes like this :

```php
use Diamond\JEloquent\PersianDateTrait;
use Illuminate\Database\Eloquent\Model;

class User extends Model {
    use PersianDateTrait;

    /**
    * Add this method to customize your date attributes
    *
    * @return array
    */
    public function getDates()
    {
        return ['created_at', 'updated_at', 'expired_at'];
    }
    
}

# or

class User extends Model {
    use PersianDateTrait;

    /**
     * The attributes that should be mutated to dates.
     *
     * @var array
     */
    protected $dates = ['created_at', 'updated_at', 'expired_at'];
}
```
When using the above trait all of the fields that are treated like date objects by Laravel will be available for conventional converting. They will be also added to model's ```toJson()``` , ```toArray();``` and ```__toString();``` methods.

##### Converter helper method

The ```$model->convertToPersian($attribute, $format);``` method allowes you to normally convert one of your fields, from Gregorian date to Jalali date :

```php
$user = Auth::user();
$user->convertToPersian('created_at', 'y/m/d'); // 93/09/08
```


