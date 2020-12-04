Example of an envoy proxy set up that implements a JWT authentication and rate limiting using the payload of the token.

The configuration expects a signed JWT token to be included in the Authorization header of the HTTP request.

Once the token is validated the request is ratelimited using the paylod of the token. To be effective the token must include a unique value either for the user or the token itself. It is suggested that this is done by including a *jti* claim containing a UUID, or alternatively a *sub* subject claim for the user.

In the example the shared secret used is TEST_SECRET_JKS_DO_NOT_USE_IN_LIVE

Example header
`
{
  "alg": "HS256",
  "typ": "JWT"
}
`
Example payload
`
{
  "jti": "427e1f50-03c8-45f4-8e79-d0d3e1350623",
  "iss": "eq",
  "exp": 2516239022
}
`

Example token

Ratelimiting is performed using envoys external rate limit service backed by redis.

To run the example...

`
docker-compose up -d
`

To perform a successful request
`
curl -i -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI0MjdlMWY1MC0wM2M4LTQ1ZjQtOGU3OS1kMGQzZTEzNTA2MjMiLCJpc3MiOiJlcSIsImV4cCI6MjUxNjIzOTAyMn0.xZGMLRFImFj3spewQ2qEtvaPHbhyHGfC8uKFjc-hIbk" http://localhost:10000/
`

