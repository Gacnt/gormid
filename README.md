# gormid
Use [GORM](https://github.com/jinzhu/gorm) (Go Object Relational Mapping) to store OpenID DiscoveryCache / Nonce in a database instead of in memory

# Installation
`go get github.com/Gacnt/gormid`


# Usage
NOTE: IF USING MYSQL

In order to handle `time.Time`, you need to include parseTime as a parameter. (THIS MUST BE IMPLEMENTED OR PACKAGE WILL PANIC) 

e.g. `db, err := gorm.Open("mysql", "user:password@/dbname?charset=utf8&parseTime=True&loc=Local")`



```
import "github.com/Gacnt/gormid"

 
// Pass in your *gorm.DB and then access the DiscoveryCache and NonceStore 
// fields of the struct returned e.g. as well, this will create the required tables in
// your database so you don't need to worry about that

var gormStore = gormid.CreateNewStore(db) 

func AuthCallback(w http.ResponseWriter, r *http.Request) {
	fullURL := "http://localhost:3000" + r.URL.String()
	log.Println(fullURL)
	id, err := openid.Verify(
		fullURL,
		gormStore.DiscoveryCache, gormStore.NonceStore)
	if err == nil {
		log.Println(id)
	} else {
		log.Println(err)
	}
}
```

#Docs:
https://godoc.org/github.com/Gacnt/gormid

#Testing
Tests are copied from [openid-go](https://github.com/yohcop/openid-go) and as of now they both pass just fine.
`go test` 
They are currently set to run with `postgres` and a default user of `postgres` and on database `pugit` feel free to modify the test files DB to run tests to your liking


