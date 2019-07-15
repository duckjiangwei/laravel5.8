Application类的数据存储  
```php  
instances=[
    'path'=>'app',
    'path.base'=>'\',
    'path.lang'=>'resoruces\lang',
    'path.config'=>'config',
    'path.public'=>'public',
    'path.storage'=>'storage',
    'path.database'=>'database',
    'path.resources'=>'resources',
    'path.bootstrap'=>'bootstrap',
    'app'=>'Application实例',
    'Illuminate\Container\Container::class'=>'Application实例',
    'Illuminate\Foundation\PackageManifest::class'=>'Illuminate\Foundation\PackageManifest实例',
    /**
    new PackageManifest(
                new Filesystem, $this->basePath(), $this->getCachedPackagesPath()
            )
    **/ 
    'app'=>'Application实例',
    'request'=>Illuminate\Http\Request实例,
    'config'=>Illuminate\Config\Repository实例,
    'routes'=>Illuminate\Routing\RouteCollection实例,
    'routes'=>Illuminate\Routing\RouteCollection实例,
]

bootedCallbacks=>[
            'function () {
                              $this->app['router']->getRoutes()->refreshNameLookups();
                              $this->app['router']->getRoutes()->refreshActionLookups();
                          }',
]

reboundCallbacks=[
    'request'=>'function ($app, $request) {
                            $app['url']->setRequest($request);
                        }',
      
    'routes'=>'function ($app, $routes) {
                               $app['url']->setRoutes($routes);
                           }',                  
]


bindings=[
    'events'=>'function ($app) {
                           return (new Dispatcher($app))->setQueueResolver(function () use ($app) {
                               return $app->make(QueueFactoryContract::class);
                           });
                       }',
                       
    'log'=>'function () {
                        return new LogManager($this->app);
                    }',  
                    
    'router'=>'function ($app) {
                           return new Router($app['events'], $app);
                       }',
                       
    'url'=>'function ($app) {
                        $routes = $app['router']->getRoutes();
        
                        $app->instance('routes', $routes);
            
                        $url = new UrlGenerator(
                            $routes, $app->rebinding(
                                'request', $this->requestRebinder()
                            ), $app['config']['app.asset_url']
                        );
  
                        $url->setSessionResolver(function () {
                            return $this->app['session'] ?? null;
                        });
            
                        $url->setKeyResolver(function () {
                            return $this->app->make('config')->get('app.key');
                        });
    
                        $app->rebinding('routes', function ($app, $routes) {
                            $app['url']->setRoutes($routes);
                        });
            
                        return $url;
                    }',    
                    
    'redirect'=>'function ($app) {
                             $redirector = new Redirector($app['url']);
    
                             if (isset($app['session.store'])) {
                                 $redirector->setSession($app['session.store']);
                             }
                 
                             return $redirector;
                         }',      
                         
    'ServerRequestInterface::class'=>'function ($app) {
                                                  return (new DiactorosFactory)->createRequest($app->make('request'));
                                              }',  
                                              
    'ResponseInterface::class'=>'function () {
                                             return new PsrResponse;
                                         }',  
                                         
    'Illuminate\Contracts\Routing\ResponseFactory'=>'function ($app) {
                                                   return new ResponseFactory($app[ViewFactoryContract::class], $app['redirect']);
                                               }',   
                                               
    'Illuminate\Routing\Contracts\ControllerDispatcher'=>'function ($app) {
                                                                      return new ControllerDispatcher($app);
                                                                  }',   
                                                                      
    'Illuminate\Contracts\Http\Kernel::class'=>'function ($container, $parameters = []) use (Illuminate\Contracts\Http\Kernel::class, App\Http\Kernel::class) {
                                              if ($abstract == $concrete) {
                                                  return $container->build($concrete);
                                              }
                                  
                                              return $container->resolve(
                                                  $concrete, $parameters, $raiseEvents = false
                                              );
                                          }',    
                                          
    'Illuminate\Contracts\Console\Kernel::class'=>'function ($container, $parameters = []) use (Illuminate\Contracts\Console\Kernel::class, App\Console\Kernel::class) {
                                                  if ($abstract == $concrete) {
                                                      return $container->build($concrete);
                                                  }
                                      
                                                  return $container->resolve(
                                                      $concrete, $parameters, $raiseEvents = false
                                                  );
                                              }',   
    'Illuminate\Contracts\Debug\ExceptionHandler::class'=>'function ($container, $parameters = []) use (Illuminate\Contracts\Debug\ExceptionHandler::class, App\Exceptions\Handler::class) {
                                                      if ($abstract == $concrete) {
                                                          return $container->build($concrete);
                                                      }
                                          
                                                      return $container->resolve(
                                                          $concrete, $parameters, $raiseEvents = false
                                                      );
                                                  }', 
                                                  
    'Illuminate\Contracts\Debug\ExceptionHandler::class'=>'function ($container, $parameters = []) use (Illuminate\Contracts\Debug\ExceptionHandler::class, App\Exceptions\Handler::class) {
                                                          if ($abstract == $concrete) {
                                                              return $container->build($concrete);
                                                          }
                                              
                                                          return $container->resolve(
                                                              $concrete, $parameters, $raiseEvents = false
                                                          );
                                                      }',                                                                                           
                                                                                                                                                                                                                                                                                                                                                
]

aliases=[
          Illuminate\Contracts\Container\Container] => app
          [Illuminate\Contracts\Foundation\Application] => app
          [Psr\Container\ContainerInterface] => app
          [Illuminate\Auth\AuthManager] => auth
          [Illuminate\Contracts\Auth\Factory] => auth
          [Illuminate\Contracts\Auth\Guard] => auth.driver
          [Illuminate\View\Compilers\BladeCompiler] => blade.compiler
          [Illuminate\Cache\CacheManager] => cache
          [Illuminate\Contracts\Cache\Factory] => cache
          [Illuminate\Cache\Repository] => cache.store
          [Illuminate\Contracts\Cache\Repository] => cache.store
          [Illuminate\Config\Repository] => config
          [Illuminate\Contracts\Config\Repository] => config
          [Illuminate\Cookie\CookieJar] => cookie
          [Illuminate\Contracts\Cookie\Factory] => cookie
          [Illuminate\Contracts\Cookie\QueueingFactory] => cookie
          [Illuminate\Encryption\Encrypter] => encrypter
          [Illuminate\Contracts\Encryption\Encrypter] => encrypter
          [Illuminate\Database\DatabaseManager] => db
          [Illuminate\Database\Connection] => db.connection
          [Illuminate\Database\ConnectionInterface] => db.connection
          [Illuminate\Events\Dispatcher] => events
          [Illuminate\Contracts\Events\Dispatcher] => events
          [Illuminate\Filesystem\Filesystem] => files
          [Illuminate\Filesystem\FilesystemManager] => filesystem
          [Illuminate\Contracts\Filesystem\Factory] => filesystem
          [Illuminate\Contracts\Filesystem\Filesystem] => filesystem.disk
          [Illuminate\Contracts\Filesystem\Cloud] => filesystem.cloud
          [Illuminate\Hashing\HashManager] => hash
          [Illuminate\Contracts\Hashing\Hasher] => hash.driver
          [Illuminate\Translation\Translator] => translator
          [Illuminate\Contracts\Translation\Translator] => translator
          [Illuminate\Log\LogManager] => log
          [Psr\Log\LoggerInterface] => log
          [Illuminate\Mail\Mailer] => mailer
          [Illuminate\Contracts\Mail\Mailer] => mailer
          [Illuminate\Contracts\Mail\MailQueue] => mailer
          [Illuminate\Auth\Passwords\PasswordBrokerManager] => auth.password
          [Illuminate\Contracts\Auth\PasswordBrokerFactory] => auth.password
          [Illuminate\Auth\Passwords\PasswordBroker] => auth.password.broker
          [Illuminate\Contracts\Auth\PasswordBroker] => auth.password.broker
          [Illuminate\Queue\QueueManager] => queue
          [Illuminate\Contracts\Queue\Factory] => queue
          [Illuminate\Contracts\Queue\Monitor] => queue
          [Illuminate\Contracts\Queue\Queue] => queue.connection
          [Illuminate\Queue\Failed\FailedJobProviderInterface] => queue.failer
          [Illuminate\Routing\Redirector] => redirect
          [Illuminate\Redis\RedisManager] => redis
          [Illuminate\Contracts\Redis\Factory] => redis
          [Illuminate\Http\Request] => request
          [Symfony\Component\HttpFoundation\Request] => request
          [Illuminate\Routing\Router] => router
          [Illuminate\Contracts\Routing\Registrar] => router
          [Illuminate\Contracts\Routing\BindingRegistrar] => router
          [Illuminate\Session\SessionManager] => session
          [Illuminate\Session\Store] => session.store
          [Illuminate\Contracts\Session\Session] => session.store
          [Illuminate\Routing\UrlGenerator] => url
          [Illuminate\Contracts\Routing\UrlGenerator] => url
          [Illuminate\Validation\Factory] => validator
          [Illuminate\Contracts\Validation\Factory] => validator
          [Illuminate\View\Factory] => view
          [Illuminate\Contracts\View\Factory] => view
          ]  
          
          
          
abstractAliases= [
 [app] => Array
         (
             [0] => 
             [1] => Illuminate\Contracts\Container\Container
             [2] => Illuminate\Contracts\Foundation\Application
             [3] => Psr\Container\ContainerInterface
         )
 
     [auth] => Array
         (
             [0] => Illuminate\Auth\AuthManager
             [1] => Illuminate\Contracts\Auth\Factory
         )
 
     [auth.driver] => Array
         (
             [0] => Illuminate\Contracts\Auth\Guard
         )
 
     [blade.compiler] => Array
         (
             [0] => Illuminate\View\Compilers\BladeCompiler
         )
 
     [cache] => Array
         (
             [0] => Illuminate\Cache\CacheManager
             [1] => Illuminate\Contracts\Cache\Factory
         )
 
     [cache.store] => Array
         (
             [0] => Illuminate\Cache\Repository
             [1] => Illuminate\Contracts\Cache\Repository
         )
 
     [config] => Array
         (
             [0] => Illuminate\Config\Repository
             [1] => Illuminate\Contracts\Config\Repository
         )
 
     [cookie] => Array
         (
             [0] => Illuminate\Cookie\CookieJar
             [1] => Illuminate\Contracts\Cookie\Factory
             [2] => Illuminate\Contracts\Cookie\QueueingFactory
         )
 
     [encrypter] => Array
         (
             [0] => Illuminate\Encryption\Encrypter
             [1] => Illuminate\Contracts\Encryption\Encrypter
         )
 
     [db] => Array
         (
             [0] => Illuminate\Database\DatabaseManager
         )
 
     [db.connection] => Array
         (
             [0] => Illuminate\Database\Connection
             [1] => Illuminate\Database\ConnectionInterface
         )
 
     [events] => Array
         (
             [0] => Illuminate\Events\Dispatcher
             [1] => Illuminate\Contracts\Events\Dispatcher
         )
 
     [files] => Array
         (
             [0] => Illuminate\Filesystem\Filesystem
         )
 
     [filesystem] => Array
         (
             [0] => Illuminate\Filesystem\FilesystemManager
             [1] => Illuminate\Contracts\Filesystem\Factory
         )
 
     [filesystem.disk] => Array
         (
             [0] => Illuminate\Contracts\Filesystem\Filesystem
         )
 
     [filesystem.cloud] => Array
         (
             [0] => Illuminate\Contracts\Filesystem\Cloud
         )
 
     [hash] => Array
         (
             [0] => Illuminate\Hashing\HashManager
         )
 
     [hash.driver] => Array
         (
             [0] => Illuminate\Contracts\Hashing\Hasher
         )
 
     [translator] => Array
         (
             [0] => Illuminate\Translation\Translator
             [1] => Illuminate\Contracts\Translation\Translator
         )
 
     [log] => Array
         (
             [0] => Illuminate\Log\LogManager
             [1] => Psr\Log\LoggerInterface
         )
 
     [mailer] => Array
         (
             [0] => Illuminate\Mail\Mailer
             [1] => Illuminate\Contracts\Mail\Mailer
             [2] => Illuminate\Contracts\Mail\MailQueue
         )
 
     [auth.password] => Array
         (
             [0] => Illuminate\Auth\Passwords\PasswordBrokerManager
             [1] => Illuminate\Contracts\Auth\PasswordBrokerFactory
         )
 
     [auth.password.broker] => Array
         (
             [0] => Illuminate\Auth\Passwords\PasswordBroker
             [1] => Illuminate\Contracts\Auth\PasswordBroker
         )
 
     [queue] => Array
         (
             [0] => Illuminate\Queue\QueueManager
             [1] => Illuminate\Contracts\Queue\Factory
             [2] => Illuminate\Contracts\Queue\Monitor
         )
 
     [queue.connection] => Array
         (
             [0] => Illuminate\Contracts\Queue\Queue
         )
 
     [queue.failer] => Array
         (
             [0] => Illuminate\Queue\Failed\FailedJobProviderInterface
         )
 
     [redirect] => Array
         (
             [0] => Illuminate\Routing\Redirector
         )
 
     [redis] => Array
         (
             [0] => Illuminate\Redis\RedisManager
             [1] => Illuminate\Contracts\Redis\Factory
         )
 
     [request] => Array
         (
             [0] => Illuminate\Http\Request
             [1] => Symfony\Component\HttpFoundation\Request
         )
 
     [router] => Array
         (
             [0] => Illuminate\Routing\Router
             [1] => Illuminate\Contracts\Routing\Registrar
             [2] => Illuminate\Contracts\Routing\BindingRegistrar
         )
 
     [session] => Array
         (
             [0] => Illuminate\Session\SessionManager
         )
 
     [session.store] => Array
         (
             [0] => Illuminate\Session\Store
             [1] => Illuminate\Contracts\Session\Session
         )
 
     [url] => Array
         (
             [0] => Illuminate\Routing\UrlGenerator
             [1] => Illuminate\Contracts\Routing\UrlGenerator
         )
 
     [validator] => Array
         (
             [0] => Illuminate\Validation\Factory
             [1] => Illuminate\Contracts\Validation\Factory
         )
 
     [view] => Array
         (
             [0] => Illuminate\View\Factory
             [1] => Illuminate\Contracts\View\Factory
         )
 ]                          
```  

Illuminate\Routing\Router的数据存储   
```php  
middlewareGroups=[
'web' => [
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            // \Illuminate\Session\Middleware\AuthenticateSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],

        'api' => [
            'throttle:60,1',
            'bindings',
        ],
]

middleware=[
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
        'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
]

protected $events;


protected $container;


protected $routes;


protected $current;


protected $currentRequest;



public $middlewarePriority = [];


protected $binders = [];


protected $patterns = [];

protected $groupStack = [
    0=>[
        'middleware'=>[
                0=>'web'
        ],
        namespace='App\Http\Controllers'
    ]
];

public static $verbs = ['GET', 'HEAD', 'POST', 'PUT', 'PATCH', 'DELETE', 'OPTIONS'];
```