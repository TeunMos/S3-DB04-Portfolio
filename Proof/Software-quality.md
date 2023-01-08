
# Software quality
This file containes all the proof for the Software quality learing outcome.
> **Tooling and methodology:** Carry out, monitor and report on unit integration, regression and system tests, with attention for security and performance aspects, as well as applying static code analysis and code reviews.

## Sonarcloud
For both the group project and the individual project we use sonarcloud to review our code quality and coverage on our tests. We decided to use sonarcloud because it is easy to set up, it is widely used in practice and because we needed a way to automatically test our code.

Here are the links to the Sonarcloud organisations:
- [Sonarcloud group project](https://sonarcloud.io/organizations/modus-1/projects)
- [Sonarcloud individual project](https://sonarcloud.io/organizations/ips3-db04-teun-mos-lukas-jansen/projects)

### Quality gate conditions
For a Sonarcloud quality gate to pass you need to set some conditions first.

These are the conditions for our **group project**'s quality gate to ***fail***:
|Metric|Operator|Value|
|--|--|--|
|~~Coverage~~|~~is less than~~|~~80.0%~~|
|Duplicated Lines (%)|is greater than|3.0%|
|Maintainability Rating|is worse than|A|
|Reliability Rating|is worse than|A|
|Security Hotspots Reviewed|is less than|100%|
|Security Rating|is worse than|A|
> this info may be out of date,  to ensure you have the latest information [*click here!*](https://sonarcloud.io/organizations/modus-1/quality_gates/show/9)

We let everything (exept for the code coverage) on the default settings. We did that because we were able to meet those conditions *(and later we found out we couldn't meet the coverage condition)*. 

For our group project we decided to remove the 80% code coverage requirement because we found out that that is not pracital for every repository, now we look what needs to be tested the most instead of a fixed value that determines if you have tested enough.

## Group project

### branches
For our group project we protected the "main" and "development" branches on our repositories. This means you need to create a pull request to merge into those branches.
For our group we have made it so you have to meet a few requirements before a pull request can be approved and merged:
- The pull request must be approved by at least 2 other group members.
- The code must have a succesful build.
- The Sonarcloud quality check must pass.

### Unit tests
We created a lot of tests for our repositories in the group project, but the tests i've made myself (together with [Lukas](https://github.com/LukasJansen100)) are mainly in the menu-api repository which you an check out [*over here!*](https://github.com/Modus-1/menu-api)

## Individual project
### branches
Just as we did for our group project, our individual project has the "main" branch protected, so you need to make a pull request to merge into those branches.
For our individual project we also made a few requirements before a pull request can be approved and merged, because i'm doing the project with [Lukas](https://github.com/LukasJansen100) we can't make it so 2 people need to approve a pull request. 
but apart from that the requirements before a pull request can be approved are the same:
- The pull request must be approved by the other member.
- The code must have a succesful build.
- The Sonarcloud quality check must pass.

We did that because this made sure that the code has less bugs and stays at a high quality level.

#### sonarcloud
we have decided to not write down specific quality gate conditions for our individual project, but we try to keep the code tidy by always looking at all the code smells and security vulnerabilities when we make a pull request. If sonarcloud detects any, we will fix them first before merging. We did that because we have a smaller team, but we still look at what sonarcloud says and ask each other if it is necessary do something if sonarcloud finds something.



### Unit-tests
Unit/integration tests are really important to make sure the code works as intended. We test our code based on the user-stories, to make sure a user-story is complete

#### User-preferences-API
For our individual project we started working on unit tests for our [*User-Preferences-API*](https://github.com/IPS3-DB04-Teun-Mos-Lukas-Jansen/User-Preferences-API). We created this api in python (which we have never used before), so we first needed to find out how to write unit-tests for python. Eventually we found a package named *[PyTest](https://pytest.org/)* which works well for our project. For mocking we couldn't find a lot of packages that work for our project, but to mock our database we use [*mongomock*](https://github.com/mongomock/mongomock). We didn't go the traditional route with mocking our API, we imported our script and changed the variables that are used to our mocked ones.

here is an example of how we mock our Mongodb Database for the testing of our authentication library:
``` python
import auth #our authentication library
import mongomock

def init_mock_layout_db(dummy_card_id, dummy_user_id):
	collection_mock = mongomock.MongoClient().mocklayout_db.mocklayout_collection #creates a mocked mongodb collection running in memory
	auth.layoutdb = collection_mock #sets the database used in the authentication library to our mocked one by changing the 'layoutdb' variable.

	card_obj = Card(cardId= dummy_card_id, cardType="url") # inserts a card in the layout for testing
	collection_mock.update_one({"userId": dummy_user_id } ,{'$push': {'columns.0.cards': dict(card_obj)}}, upsert = True )
```

This is how one of our tests looks like for our authentication library:
``` python
import auth #our authentication library we are testing

def test_verify_urlcard_to_user_when_token_exists_should_return_true():
    #arrange
    user_id = str(uuid.uuid4()) 
    card_id = str(uuid.uuid4())
    mock_init(user_id)
    init_mock_layout_db(card_id, user_id)
    dummy_token = "dummy_token"

    #act
    result = auth.verify_urlcard_to_user(dummy_token, card_id) # this function checks if the token is correct and returns true if it is

    #assert
    assert result == True 
```

### Integration-tests
> Integration-tests are the step after unit-tests and is the phase in testing in which individual software modules are combined and tested as a group.
> [Wikipedia](https://en.wikipedia.org/wiki/Integration_testing#:~:text=Integration%20testing%20%28sometimes%20called%20integration%20and%20testing,%20abbreviated%20I&T%29%20is%20the%20phase%20in%20software%20testing%20in%20which%20individual%20software%20modules%20are%20combined%20and%20tested%20as%20a%20group)
#### Integration-API
For our integration-API we decided to test our endpoints and validate the data is in a format that our front-end understands.
We are using Xunit as our testing framework.
To start our whole project and mocking the data-layer we created a **web application factory** with the help of [this page](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests?view=aspnetcore-7.0#customize-webapplicationfactory). Apart form that the tests look a bit like unit tests.

here is an example of a test we've made:
``` c#
        [Fact]
        public async Task GetUserCredentials_returnsSuccess()
        {
            //arrange
            string id_token_dummy = "1234";
            string user_id_dummy = "4321";
            MockAuth(id_token_dummy, user_id_dummy);

            var mockCredentialsDataAcces = new Mock<ICredentialsDataAcces>();
            _factory.MockCredentialsDataAcces(mockCredentialsDataAcces.Object);
            mockCredentialsDataAcces.Setup(x => x.GetAllCredentials(user_id_dummy)).ReturnsAsync(_credentials_dummy);

            var client = _factory.CreateClient(new WebApplicationFactoryClientOptions
            {
                AllowAutoRedirect = false
            });

            //act
            var response = await client.GetAsync($"/credentials/{id_token_dummy}");

            //assert

            //  ensure status code 200 OK
            response.EnsureSuccessStatusCode();
            Assert.Equal(HttpStatusCode.OK, response.StatusCode);

            //  ensure response body is correct
            var result = await response.Content.ReadAsStringAsync();
            JArray jArray = JArray.Parse(result);
            Assert.Equal("integration1", jArray[0]["name"].ToString());
            Assert.Equal("integration2", jArray[1]["name"].ToString());
            Assert.True(jArray[0]["credentials"]["Active"].ToObject<bool>());
            Assert.False(jArray[1]["credentials"]["Active"].ToObject<bool>());
        }
```
this test calls the "*/credentials*" endpoint with an id_token from google. It checks if the correct credentials are returned in a format that the front-end understands.

