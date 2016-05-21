# FirebaseDatabase.net
[![AppVeyor Build status](https://ci.appveyor.com/api/projects/status/ep8xw22cexktghba?svg=true)](bezysoftware/firebase-database-dotnet)

Simple wrapper on top of Firebase Database REST API. Among others it supports streaming API which you can use for realtime notifications.

## Installation
```csharp
// Install pre-release version
Install-Package FirebaseDatabase.net -pre
```

## Usage

### Querying

```csharp
var firebase = new FirebaseClient("https://dinosaur-facts.firebaseio.com/");
var dinos = firebase
  .Child("dinosaurs")
  .OrderByKey()
  .StartAt("pterodactyl")
  .LimitToFirst(2)
  .OnceAsync<Dinosaur>();
```

### Saving data

```csharp
var firebase = new FirebaseClient("https://dinosaur-facts.firebaseio.com/");

// add new item to list of data and let the client generate new key for you (done offline)
var dino = firebase
  .Child("dinosaurs")
  .Post(new Dinosaur());
  
Console.WriteLine($"Key for the new dinosaur: {dino.Key}");  

// add new item directly to the specified location (this will overwrite whatever data already exists at that location)
var dino = firebase
  .Child("dinosaurs")
  .Child("t-rex")
  .Put(new Dinosaur());

```

### Realtime streaming

```csharp
var firebase = new FirebaseClient("https://dinosaur-facts.firebaseio.com/");
var observable = firebase
  .Child("dinosaurs")
  .AsObservable<Dinosaur>()
  .Subscribe(d => Console.WriteLine(d.Key));
  
```

```AsObservable<T>``` methods returns an ```IObservable<T>``` which you can take advantage of using [Reactive Extensions](https://github.com/Reactive-Extensions/Rx.NET)
