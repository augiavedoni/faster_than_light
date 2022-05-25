# Faster Than Light RESTful API

## About

This RESTful API aims to simulate spaceships fighting against each other. Some restrictions and game rules were inspired by the videogame [FTL: Faster Than Light](https://store.steampowered.com/app/212680/FTL_Faster_Than_Light/). This project is created as requested by the pre-training program dictated by Code Sherpas.

**IMPORTANT:** at the moment, this API is under development so the features present are not definitive and they may be subject to change.

## Spaceship model

Each spaceship is made up of the following attributes:

  - ID: long
  - Name: string
  - Health: int

## Endpoints

Using the command `curl http://localhost:8080/api/{endpoint}` you can interact with the API. The avaible endpoints are the following:

1. **POST /spaceships**: this endpoint expects you to send as the body of the request the information about a spaceship. The estructure of the model that represents a spaceship was explained earlier. It returns the spaceship information that was added to the database. For example:
```
curl http://localhost:8080/api/spaceships \
    --include \
    --header "Content-Type: application/json" \
    --request "POST" \
    --data '{"name": "Destructor","health": 100}'
```
**IMPORTANT:** there are some things to consider about this particular endpoint. They're as follow:

  * The name field is mandatory. It can't be null or empty.
  * The health field is mandatory. It can't be null and it can't be zero or less.

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

### Hope you like it and have fun :)
