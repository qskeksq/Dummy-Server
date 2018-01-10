# Dummy-Server
서버코드보다 자바스크립트 코드 사용법을 공부해본다


## Dummy Database

```javaScript
var DummyDB = function(){
    // 변수 선언
    var count = 1;
    var storage = [];
    var DummyDB = {};

    DummyDB.get = function(id){

        if(id){
            // 아이디가 String 값이면 숫자로 바꾸고 숫자이면 그냥 숫자 리턴
            id = (typeof id == 'string') ? Number(id) : id;

            // 저장소 전체를 돌면서
            for(var i in storage){
                // 아이디 값이 일치하면 하나를 리턴하고
                if(storage[i].id == id){
                    return storage[i];
                    // 그렇지 않으면 전체를 리턴한다
                } 
            }
        } else {
            return storage;
        }
    }

    DummyDB.insert = function(data){
        data.id = count++;
        storage.push(data);
        return data;
    };

    DummyDB.remove = function(id){
        id = (typeof id == 'string') ? Number(id) : id;

        for(var i in storage){
            if(storage[i].id == id){
                storage.splice(i, 1);
                return true;
            }
        }
        return false;
    }
    return DummyDB;
}();
```


## CRUD

- create
    ```javaScript
    app.post('/user', (request, response)=>{

        var name = request.body.name;
        var region = request.body.region;

        // 유효성 검사
        if(name && region){
            response.send(DummyDB.insert({
                name : name,
                region :region
            }));
        } else {
            throw new Error('error');
        }
    })
    ```

- read
    ```javaScript
    app.get('/user', (request, response)=>{
        response.send(DummyDB.get());
    })
    ```
    ```javaScript
    app.get('/user/:id', (request, response)=>{
        response.send(DummyDB.get(request.params.id));
    })
    ```       
    ```javaScript
    app.put('/user/:id', (request, response)=>{
        var id = request.params.id;
        var name = request.body.name;
        var region = request.body.region;

        var item = DummyDB.get(id);
        item.name = name || item.name;
        item.region = region || item.region;

        response.send(item);

    })
    ```
    
- delete
    ```javaScript
    app.delete('/user/:id', (request, response)=>{
        response.send(DummyDB.remove(request.params.id));
    })
    ```
