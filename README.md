# Faster Than Light RESTful API

## About

This RESTful API aims to simulate spaceships fighting against each other. Some restrictions and game rules were inspired by the videogame [FTL: Faster Than Light](https://store.steampowered.com/app/212680/FTL_Faster_Than_Light/).

**IMPORTANT:** at the moment, this API is under development so the features present are not definitive and they may be subject to change. Besides that, you may encounter bugs or things that can be polished. If you want to help, don't doubt it and submit a PR.

## Current CI/CD pipeline status

[![CI/CD Pipeline](https://github.com/augiavedoni/faster_than_light/actions/workflows/java.yml/badge.svg)](https://github.com/augiavedoni/faster_than_light/actions/workflows/java.yml)

## Spaceship model

Each spaceship is made up of the following attributes:

  - ID: long
  - Name: string
  - Health: int
  - Weapon: Weapon
  - PowerGenerator: PowerGenerator

### Spaceship as JSON

```javascript
{
  "id": 1,
  "name": "Destructor",
  "health": 100,
  "weapon": {
    "id": 1,
    "weapon-power-needed": 5
  },
  "power-generator": {
    "id": 1,
    "total-power": 10,
    "power-not-in-use": 5,
    "power-consumed-by-weapon": 5
  }
}
```

## Endpoints

Using the command `curl http://localhost:8080/api/{endpoint}` you can interact with the API. The avaible endpoints are the following:

1. **POST /spaceships**: this endpoint expects you to send as the body of the request the information about a spaceship. The estructure of the model that represents a spaceship was explained earlier. It returns the spaceship information that was added to the database. For example:
```
curl http://localhost:8080/api/spaceships \
    --include \
    --header "Content-Type: application/json" \
    --request "POST" \
    --data '{"name": "Destructor","health": 100, "weapon": {"weapon-power-needed": 5}, "power-generator": {"total-power": 10, "power-consumed-by-weapon": 4}}'
```
**IMPORTANT:** there are some things to consider about this particular endpoint. They're as follow:

  * The name field is mandatory. It can't be null or empty.
  * The health field is mandatory. It can't be null and it can't be zero or less.
  * The ID of the spaceship is going to be autogenerated by the database.
  * The weapon field is mandatory as well as its inner field weapon-power-needed. Also take into consideration that it cannot be zero.
  * The power-generator field is mandatory as well as its inner fields total-power and power-consumed-by-weapon. Both cannot be negative numbers.
  * The power-consumed-by-weapon field cannot be higher than weapon-power-needed. Otherwise, you'll receive an exception.

2. **GET /spaceships**: it returns the information about all the spaceships that are present in the database. For example:
```
curl http://localhost:8080/api/spaceships
```
3. **PATCH /spaceships/shoot?attackerID={attackerId}&victimId={victimId}**: this endpoint aims to represent a spaceship shooting another one. It expects you to pass as the query parameters the ID of the spaceship that is attacking and the ID of the spaceship that is receiving the shot. For example:
```
curl --request PATCH http://localhost:8080/api/spaceships/shoot?attackerId=1&victimId=4
```
**IMPORTANT:** there are some things to consider about this particular endpoint. They're as follow:

  * A destroyed spaceship cannot shoot. This translates to the fact that the health of the spaceship that is attacking can't be 0.
  * The health of the spaceship cannot go below 0, so if a spaceship reaches that state, that spaceship is considered destroyed and you can't attack it any more.
  * The weapon on the spaceship is not going to be able to shoot if the power-consumed-by-weapon isn't the same as the weapon-power-needed field.

### Hope you like it and have fun :)
