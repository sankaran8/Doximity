# Doximity
Laravel SocialiteProviders for Doximity. For more information about `doximity` read https://www.doximity.com/developers/documentation 
## Usage
Laravel SocialiteProviders for Doximity. It helps to integrate `doximity's OAuth authorization` to your laravel application.
## Installation
1. Add `Socialite` and this Doximity api through composer
2. Add `Socialite` to Providers array in your `config/app.php` 
```php
'providers' => [
    // Other service providers...

    Laravel\Socialite\SocialiteServiceProvider::class,
],
```
Also, add the `Socialite facade` to the aliases array in your app configuration file:
```php
'Socialite' => Laravel\Socialite\Facades\Socialite::class,
```
3.Add credentials for the `Doximity OAuth services` in your `config/services.php` 
```php
'doximity' => [
    'client_id' => 'your-doximity-app-id',
    'client_secret' => 'your-doximity-app-secret',
    'redirect' => 'http://your-app-callback-url-here',
],
```
4.Next extend `Socialite` to add `Doximity` into it.
Plcae this function inside your `AppServiceProvider.php` 
```php
private function addDoximityToSocialite()
{
    $socialite = $this->app->make('Laravel\Socialite\Contracts\Factory');
    $socialite->extend(
        'doximity',
        function ($app) use ($socialite) {
            $config = $app['config']['services.doximity'];
            return $socialite->buildProvider(\Doximity\Provider::class, $config);
        }
    );
}
```
Call this function inside `boot()` of `AppServiceProvider.php` 
```php
public function boot()
{
    $socialite = $this->app->addDoximityToSocialite();
}
```
5. Create a controller for doximity login add add folowing functions
```php
public function redirectToProvider()
{
    return Socialize::driver('doximity')->redirect();
}

public function handleCallback()
{
    $user = Socialize::driver('doximity')->user();
}
```
6. Create `routes` for those methods
```php
$router->get('/login/doximity', 'App\Controllers\Auth\DoximityController@redirectToProvider')
        ->name('login.doximity');

$router->get('/login/doximity/response', 'App\Controllers\Auth\DoximityController@handleCallback')
        ->name('login.doximity.response');
```
7 Your are ready to use doximity login. For more information read [Socialite](https://github.com/laravel/socialite/) and [Doximity documentation](https://www.doximity.com/developers/documentation) 

## License

MIT (see LICENSE file)


**Free Software, Hell Yeah!**
