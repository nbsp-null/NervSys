# Nervsys

A very slight framework based on PHP7.1+ for universal API controlling.  

Requirements: PHP7.1+ and above. Any kind of web server or running under CLI mode.  

It can be used as:

    1. Normal develop framework for Web
    2. API controller for all types of Apps
    3. Client for program communication
    4. Or more...
    
As normally use, it responses one result from one method to one request, just like what we do now on an ordinary web development.  
But, it may response multiple results from multiple methods to one request, when we need it to "guess" what we need based on the data we gave.  

Don't expect too much, it is just a newborn framework though~ 

Extensions in "/ext/" makes it greater than we can image.  

Functional extensions (class) are considered to moved out to the third part to maintain. Not only extensions, but sub-projects based on NervSys are expected.  
Everyone can join the project. Ideas, codes, tests, suggests, supports, etc... And many thanks!  

## Structure:
        
      /                                 **Root directory
      ├─api.php                           Main entry
      ├─README.md                         Readme
      ├─LICENSE                           Lincese
      ├─core/                           **Core directory
      │     ├─cli/                      **CLI working directory
      │     │    └─logs/                **CLI logging directory
      │     ├─ctr/                      **Controller directory
      │     │    ├─os/                  **OS controller directory
      │     │    │   ├─linux.php          Linux Controller
      │     │    │   ├─winnt.php          WinNT Controller
      │     │    │   └─(need more...)     Need more controllers
      │     │    ├─router/              **Router directory
      │     │    │       ├─cgi.php        CGI execution script
      │     │    │       └─cli.php        CLI execution script
      │     │    ├─os.php                 Main OS controller
      │     │    └─router.php             Main Router controller
      │     ├─cfg.ini                     Config file for CLI executable command
      │     └─cfg.php                     Config file for core system
      └─ext/                            **extension directory
           ├─font/                      **font directory
           ├─lib/                       **extension interface directory
           │    └─keys.php                Cryption Key generator interface
           ├─upload/                    **Upload extension related directory
           │       ├─en-US/             **Upload language folder (en-US)
           │       ├─zh-CN/             **Upload language folder (zh-CN)
           │       └─upload.ini           Upload error code file
           ├─authcode.php                 Auth Code extension
           ├─crypt.php                    Encrypt/decrypt extension
           ├─errno.php                    Error code extension
           ├─file.php                     Filesystem related IO extension
           ├─http.php                     HTTP request extension
           ├─image.php                    Image processing extension
           ├─keygen.php                   Cryption Key generator extension
           ├─lang.php                     Language pack extension
           ├─mpc.php                      Multi-Process Controller Extension
           ├─pdo.php                      PDO connector extension
           ├─pdo_mysql.php                MySQL extension for PDO
           ├─redis.php                    Redis connector extension
           ├─redis_lock.php               Lock extension on Redis
           ├─redis_queue.php              Queue extension on Redis
           ├─redis_session.php            Session extension on Redis
           ├─sock.php                     Socket extension
           ├─upload.php                   Upload extension
           └─...                          There will be more in the near future

Files of a project should be better containing just in one folder right under the ROOT folder.  
Files inside a project can be placed as will.  

Some example structures:

    root/                       **Root directory
        ├─PR_1/                 **Project 1 folder
        │     ├─ctr/            **Controller folder
        │     │    ├─a.php        a script
        │     │    └─b.php        b script
        │     ├─lib/            **library folder
        │     │    ├─a.php        a script
        │     │    └─b.php        b script
        │     ├─exe/            **executable program folder 
        │     │    ├─c.php        c script
        │     │    └─xxx          xxx executable program
        │     ├─.../            **Other folders containing functional scripts
        │     │    └─....php      Model ... script
        │     └─cfg.php           Config file for Project 1
        └─PR_2/                 **Project 2 folder
              ├─model_a.php       Model a script
              ├─model_b.php       Model b script
              ├─model_c.php       Model c script
              ├─....php           Model ... script
              └─cfg.php           Config file for Project 2
    
    
All script should under the right namespace for better calling by NervSys API.  

## Example:

    root/                       **Root directory
        └─pr_1/                 **Project 1 folder
              ├─ctr/            **Controller folder
              │    └─test_1.php   test 1 script
              ├─xxx/            **Controller folder
              │    └─xxx.php      test 1 script
              ├─test_2.php        test 2 script
              └─cfg.php           Config file for Project 1


****Format for test_1.php:** 
        
    //The right namespace follows the path structure
    namespace pr_1\ctr;
        
    //Any other extensions and namespaces can be used here
    use ext\http;
        
    //Class name should be exactly the same as the file name
    class test_1
    {
        /**
        * Important!!!
        *
        * This is the TrustZone config for NervSys API.
        * The keys should be function names which we want them to be called by API,
        * while the values should be the data MUST be sent to the function.
        * Don't put optional data in TrustZone values, 
        * otherwise, API will ignore the request if optional data is not passed.
        
        * All callable functions should be public.
        */
        public static $tz = [
            test_a = [a, b, c],
            test_b = [b, c],
            test_c = [c]
        ];
        
        public static function test_a()
        {
            /**
            * This function must need variables [a, b, c]
            * We can fetch the data from router::$data['a'], router::$data['b'], ...
            * The returned value will be captured by router
            * stored in router::$result['namespace/class_name/function_name']
            */
            ... (Some code)
            return something;
        }
        
        public static function test_b()
        {
            /**
            * This function must need variables [b, c], variable [d] is optional
            * Just use router::$data['d'] if exists as it is optional
            */
            ... (Some code)
            return something;
        }
        
        /**
        * A Non-Static method
        */
        public function test_c()
        {
            /**
            * This function must need variable [c], variable [d] is optional
            * Just use router::$data['d'] if exists as it is optional
            */
            ... (Some code)
            return something;
        }
    }


****Format for test_2.php:**

    //The right namespace follows the path structure
    namespace pr_1;
        
    //Any other extensions and namespaces can be used here
    use ext\sock;
        
    //Class name shoule be exactly the same as the file name
    class test_2
    {
        public static $tz = [
            test_a = [a, b, c],
            test_b = [b, c],
            test_c = []
        ];
        
        public static function test_a()
        {
            //Fetch variables
            ... (Some code)
            return something;
        }
        
        public static function test_b()
        {
            //Fetch variables
            ... (Some code)
            return something;
        }
        
        public static function test_c()
        {
            //Fetch optional variables
            ... (Some code)
            return something;
        }
    }


As said, it is an universal API controller. So, we can easily use it as follows as usual.  
It receives normal GET/POST data, and stream data in JSON format.  
Remember one param named "c" or "cmd", the two are equal.  

**Examples (using GET):**

    Usage:
        
    for test_1.php
        
    1. http://HostName/api.php&c=pr_1\ctr\test_1-test_a&a=a&b=b&c=c
    2. http://HostName/api.php&cmd=pr_1\ctr\test_1-test_a&a=a&b=b&c=c
    3. ...
        
    Above are the strict mode with detailed function name, only "test_a" is called.
        
    Let's see more:
        
    1. http://HostName/api.php&c=pr_1\ctr\test_2-test_b&a=a&b=b&c=c
    2. http://HostName/api.php&cmd=pr_1\ctr\test_2-test_b&a=a&b=b&c=c
    3. ...
        
    We called "test_b" in "pr_1\ctr\test_2" with params "b" and "c", 
    "a" is obviously usless and ignore.
        
    And there goes some interesting things, what if we do as follows?
        
    1. http://HostName/api.php&c=pr_1\ctr\test_1-test_a-test_b&a=a&b=b&c=c
    2. http://HostName/api.php&cmd=pr_1\ctr\test_1-test_a-test_b&a=a&b=b&c=c
    3. ...
        
    Right, both "test_a" and "test_b" in "pr_1\ctr\test_1" will be called 
    sharing the same data of "b" and "c", "test_a" used one more "a".
        
    This time, we do it as:
        
    http://HostName/api.php&cmd=pr_1\ctr\test_2-test_a-test_b-test_c&a=a&b=b&c=c
        
    Yep. "test_c" will run right after, as it needs no required variables.
    We now can get some compound results with differences in keys.
        
    And what if we do as follows?
        
    1. http://HostName/api.php&c=pr_1\ctr\test_1&a=a&b=b&c=c
    2. http://HostName/api.php&cmd=pr_1\ctr\test_1&a=a&b=b&c=c
    3. ...
        
    Could it be an error calling?
        
    This is called loose mode. 
    If we do this, all functions listed in TrustZone 
    will be checked with the input data structure, 
    and will be called if the structure matched or contained.
    "test_c" will always run right after, since it needs no required variables.
        
    Call order is the TrustZone list order.
        
    So, it'll be very useful to calculate in multiple algorithms 
    with one same data pack. 
        
    And more things here, we can always refresh the data structure 
    adding new data from results in our own function codes, 
    and let others be called in midway.
        
    This is more powerful than strict mode, 
    but may bring some harm if don't pay attantion on it, 
    espcially on data written. Functions will be called out of prediction.
        
    Once when we call as follows:
            
    1. http://HostName/api.php&c=pr_1\ctr\test_2-test_a&a=a&b=b
    2. http://HostName/api.php&cmd=pr_1\ctr\test_2-test_a&a=a&c=c
    3. http://HostName/api.php&cmd=pr_1\ctr\test_2-test_a&a=a&c=c&d=d&xxx=xxx...
    4. http://HostName/api.php&cmd=pr_1\ctr\test_2-test_a&whatever...(but missed some of "a", "b", "c")
        
    This won't happen because the input data structure dismatched.
    API just chooses to ignore the request to "test_a" function,
    and gives us a notice "[what] is missing" when "DEBUG" is set.
        
    And what's more:
        
    loose style:
    1. http://HostName/api.php&c=pr_1\ctr\test_1-pr_1\test_2&a=a&b=b&c=c
    2. http://HostName/api.php&cmd=pr_1\ctr\test_1-pr_1\test_2&a=a&b=b&c=c
        
    All functions that match the input data strucuture in both "pr_1\ctr\test_1" and "pr_1\test_2"
    will run. With this, we can call multiple functions in multiple modules right in one request.
    These functions share the same source data, and do their own work.
        
    strict style:
    1. http://HostName/api.php&c=pr_1\ctr\test_1-pr_1\test_2-test_a&a=a&b=b&c=c
    2. http://HostName/api.php&cmd=pr_1\ctr\test_1-pr_1\test_2-test_a-test_b&a=a&b=b&c=c
        
    Functions placed in the URL (in "c"/"cmd" value, seperated by "-", order ignored, same in "POST") 
    and match the input data strucuture at the same time in both "pr_1\ctr\test_1" and "pr_1\test_2"
    will run. With this, we can call EXACT multiple functions in EXACT multiple modules in one request.
    These modules share the same function names when exist. 
    All functions share the same source data and run with the input order.
    
**CLI Command usage:**  
    
    CLI options are as follows:
        
    c/cmd: command
    d/data: CGI data content
    p/pipe: CLI pipe content
    r/ret: process return option
    l/log: process log option (cmd, data, error, result)
    t/time: read time (in microseconds; default "0" means read till done. Works when r/ret or l/log is set)
        
    **Examples:
        
    Let's take "pr_1\ctr\test_1" as an example.
    Full command should be as some type of follows:
        
    1. /path/php api.php --ret --cmd "pr_1\ctr\test_1-test_a" --data "a=a&b=b&c=c"
    2. /path/php api.php -r -t 10000 -c "pr_1\ctr\test_1-test_b" -d "b=b&c=c"
    3. /path/php api.php -r -l -c "pr_1\ctr\test_1-test_a-test_b" -d "a=a&b=b&c=c"
    4. /path/php api.php --ret --cmd "pr_1\ctr\test_1-test_a-test_b" --data "a=a&b=b&c=c"
    5. ...
        
    JSON data package is also support as CGI mode
        
    We can also do as follows:
        
    1. /path/php api.php pr_1\ctr\test_1-test_a -d "a=a&b=b&c=c"
    2. /path/php api.php pr_1\ctr\test_1-test_b -d "b=b&c=c"
    3. /path/php api.php pr_1\ctr\test_1-test_a-test_b -d "a=a&b=b&c=c"
    4. /path/php api.php pr_1\ctr\test_1 -d "a=a&b=b&c=c"
    5. ...
        
    If we need to call external programs, make sure the "c" or "cmd" key is listing in "cfg.ini" 
    with the executable path as the value, or ever more.
        
    Something examples:
        
    "cfg.ini"
        
    mycmd = "/xxx/path/mycmd"
        
    Command:
        
    1. /path/php api.php mycmd
    2. /path/php api.php -c mycmd a b c
    3. /path/php api.php --cmd mycmd -v a -b b c
    4. ...
        
    If data needs to be sent via pipe:
        
    1. /path/php api.php mycmd -p "some data"
    2. /path/php api.php -c mycmd --pipe "some data"
    3. ...
        
    Output data will be also captured as in CGI mode and outputs via STDOUT.
        
    Don't forget to use "-r" or "--ret" to capture output data.
    If time is too short to run extenal programs, use "-t ms" or "--time ms"
        
    In CLI mode, we have 3 globle variables named "PHP_PID", "PHP_CMD" and "PHP_EXE".
        
    "PHP_PID" is the Process ID that the current PHP runs with.
    "PHP_CMD" is the Process Comand that the current script started with.
    "PHP_EXE" is php executable path, which we can execute another PHP process.
        
    All the globle variables can be fetched in "os::$env".
    
**About "cfg.php" in Project root directory**  

Each project could have a "cfg.php" as the only config file for the whole project script, in which we can set some values for extension's variables or some sepcial definitions.  
So that, the scripts in this project will run under these settings.  

For example, we can set project 1 to connect database A, but using database B in project 2; We can also set language to "en-US" in project 1, but "zh-CN" in project 2, etc...  

But, always remember, don't define same named constants in different "cfg.php"s. It'll conflict.  
All "cfg.php"s existed in the root directory of projects will be required in order right before inside script runs.  
Class variables are encouraged to use instead of definitions in "cfg.php"s.  

Some examples for "cfg.php":

    //named constants (don't conflict with other "cfg.php"s)
    define('DEF_1', 'xxxx');
    define('DEF_2', 'xxxxxxxx');
        
    //define "keygen" & "ssl_cnf" for "crypt" extension
    \ext\crypt::$keygen = '\demo\keygen';
    \ext\crypt::$ssl_cnf = '/extras/ssl/openssl.cnf';
        
    //define MySQL connection parameters for "pdo_mysql" extension
    \ext\pdo_mysql::$config['init'] = true;
    \ext\pdo_mysql::$config['host'] = '192.168.1.100';
    \ext\pdo_mysql::$config['port'] = 4000;
    \ext\pdo_mysql::$config['pwd'] = 'PASSWORD';
        
    //parameters for "errno" extension
    \ext\errno::$lang = false;
    \ext\errno::load('cars', 'errno');
        
    //parameters for "http" extension
    \ext\http::$send_payload = true;
        
    //More if needed
    ...

If you want to set all variables inside classes. That is OK, just leave the "cfg.php" files away.  

If you don't have a "cfg.php" under the root directory of the project, all settings are inherited from the one before.  

## Notice:

Once if there is only one element in router's result, it will output the inner content value in JSON and ignore the key('namespace/class_name/function_name'). 
If "DEBUG" option (in "/core/cfg.php") is set to 1 or 2, the results could be complex because one or more elements for debugging will be added to results as well.
Always remember to close "DEBUG" option (set to 0) when all are under production environment, or, the result structure will confuse us with more values inside.  

## Demos
Version 5.0.0 is on going, and not compatible with versions before.  

Demos for Ver 5.0.0 is here: [DEMO](https://github.com/Jerry-Shaw/demo). Just get it a try.  
    
## Credits
pdo_mysql Extension: [shawn](https://github.com/phpxiaowei)  
README Chinese Translation: [MileHan](https://github.com/MileHan), URL [README_zh-CN.md](https://github.com/Jerry-Shaw/NervSys/blob/master/README_zh-CN.md)

## Old Version:
Old version before 3.0.0 is discontinued and the source codes located here: [3.2.0](https://github.com/Jerry-Shaw/NervSys/tree/3.2)  

## Licensing

This software is licensed under the terms of the GPLv3.  
You can find a copy of the license in the LICENSE file.