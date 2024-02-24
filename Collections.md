## Collection 

1. collection is object of class named **"Illuminate\Support\Collection""**
   ```
    $collection = collect(['taylor', 'abigail', null])->map(function (?string $name) {
            return strtoupper($name);
        })->reject(function (string $name) {
            return empty($name);
        });
        dd(get_class($collection)); // return Illuminate\Support\Collection
        dd(gettype($collection));   //object

   ```
   ___________________
   2. Why do I use this? <br/>
   to make the array more dynamic and to facilitate updating or adding elements to it <br/>
   use to use **Chain Method** **->**<br/>
   Collection is immutable **Can't be changed** => **"Each chain creates a new instance of the collection, without changing the previous collection."** <br/>
   ```
   ->get()->map()->reject() //chain
   ```
    ___________________
   3. **whereStrict()**
      ```
      1 == "1" ; //true //not strict
      1 === "1" ; //false //strict
      ```
   ________________
4.
    ```
    $collection = collect(['Ali','Ahmed','Mustafa']);
    dd($colllection->all()); // show As array
    dd($collection());  
    ```
    _________________
  5. **Macro Collection** => Make your own collection
     ```
     Collection::macro('ToUpperCase',function(){
           return $this->map(function($name){
           return strtoupper($name);
     });
     });
     $collection = collect(['Michael', 'Tom' , "Ahmed"])->toUpperCase();
     dd($collection);
     ```
**"Note: The Macro Collection cannot be used in other methods.** If you want to use it in another method, you shouldn't rewrite it. Instead, include it in the **ServiceProvider.** <br/>
```
 public function boot(): void{
        Collection::macro('toUpperCase',function (){
            return $this->map(function ($name){
                return strtoupper($name);
            });
        });
    }
```
__________________
**Collections VS lazy collections in Laravel** <br/>
**"Lazy Collection is created to minimize the size of memory and the time taken for data returned from the database."** <br/>
**Collection puts all items in the database.** <br/>
**Lazy Collection puts only the items that you want in the database."** <br/>

____________
```
return view('index',get_defined_var()); // instead of compact or use array
```
________
**collection**
```
public function index()
    {
        $users = User::get()->filter(function($element){
            return $element->id > 500;
        });


        dump(get_class($users)); //Collection
        echo '<pre>';
        return view('index',get_defined_vars());
    }
```
_________
**Lazy Collection**
```
 $users = User::cursor()->filter(function($element){
            return $element->id > 500;
        });
```
Or
```
 $users = User::lazy()->filter(function($element){
            return $element->id > 500;
        });

```
