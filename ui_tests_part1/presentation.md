---
marp: true
theme: gaia
transition: fade
style: |
  img[alt="1"] {
    view-transition-name: one;
    contain: layout;
  }

  img:is([alt="1"], [alt="2"], [alt="3"], [alt="4"], [alt="5"]) {
    height: 64px;
    position: relative;
    top: -0.1em;
    vertical-align: middle;
    width: 64px;
  }

---

<!-- _class: lead -->

![](https://user-images.githubusercontent.com/2812510/30947310-3825724c-a433-11e7-8a0d-3c3bfe00d584.png)

# Android UI testing
#### Theory and Practice

---

# Agenda for today

- ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Deep dive into UI testing 
- ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Native Android UI Testing
- ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Mocks
- ![4](https://icongr.am/material/numeric-4-circle.svg?color=666666) Conclusion

---

<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/numeric-1-circle.svg?color=ff9900)

# Dive into UI testing 

---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

# ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Deep dive into UI testing 

Software testing pyramid

![w:900 center](images/uitestpyr.svg)

---

# ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Why do we need tests?

  * Confidence (Dont repeat your mistakes)
  * Continuous deployment 
  * Screenshots generation
  * Application assessement speed up

---

# ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Feature development process

![w:1200 center](images/feature-development-process.svg)

* Discussion of functionality
* Discussion of assessement criteria
* Feature implimentation
* Unit and UI tests implimentation
* Validation by QA

---

# ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Basic feature state machine


![w:600 center](images/basic-feature-state-machine.svg)

---

# ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Assesement creterias

* Loading
* Empty
* Reload from Empty
* Content
* Reload from Content
* Error
* Reload from Error
* Cross feature navigation
---

# ![1](https://icongr.am/material/numeric-1-circle.svg?color=666666) Abstract test case scenario

`.../featureA/assesement.yaml`
```yaml
- case: 
  - id: featureAContentIsDisplayed
  - scenario:
  Open Feature A screen
  Wait for Content
  Verify that Loading is not dispalyed  
  Verify that Error is not dispalyed
  Verify that 3 items displayed in list
  Verify 1 item title is equals "Test title 1"  
  Verify 2 item title is equals "Test title 2"  
  Verify 3 item title is equals "Test title 3"
```

---


# Agenda for today

- ![1](https://icongr.am/material/numeric-1-circle.svg?color=ff9900) Dive into UI testing 
- ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Native Android UI Testing
- ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Mocks
- ![4](https://icongr.am/material/numeric-4-circle.svg?color=666666) Conclusion

---

<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/numeric-2-circle.svg?color=ff9900)

# Native Android UI Testing

---

<style>
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}
</style>

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Native Android UI Testing

* How to write?
  * Native / Cross platform
  * Content / Screenshot
* How to run?
  * Locally / Remotely
  * Environment
* How to scale?
  * Run in parallel

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Frameworks

![w:500 center](images/types-ui-test.svg)

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Native Android Frameworks

|Espresso|Kakao|Kaspresso|
|---|---|---|
|![Espresso w:350 h:350](https://developer.android.com/static/images/training/testing/espresso.png)|![Kakao w:350 h:350](https://user-images.githubusercontent.com/2812510/30947310-3825724c-a433-11e7-8a0d-3c3bfe00d584.png)|![Kaspresso w:350 h:320](https://github.com/KasperskyLab/Kaspresso/raw/master/kaspresso.png)|

---


# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Pros and Cons (Espresso ![Espresso w:48 h:48](https://developer.android.com/static/images/training/testing/espresso.png))

|Pros|Cons|
|---|---|
|:white_check_mark: Supported by Google|:x: Lots of boilerplate|
|:white_check_mark: Native Android test library|:x: Depends on API version|
|:white_check_mark: Most popular UI testing framework|:x: Resources assertion and matching not supported out of the box|
|:white_check_mark: Fast||


---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Pros and Cons (Kaspresso ![Kaspresso w:60 h:48](https://github.com/KasperskyLab/Kaspresso/raw/master/kaspresso.png))

|Pros|Cons|
|---|---|
|:white_check_mark: Testing for ADB calls such as SMS, Calls, Geolocation and etc|:x: Requred ADB server|
|:white_check_mark: Build-in flakiness retries|:x: Tricky for scaling|
|:white_check_mark: Allure report|:x: Parallel runs on CI|
|:white_check_mark: Steps support||
|:white_check_mark: Based on Kakao||

---


# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Why Kakao?

* Espresso
```Kotlin
onView(allOf(withId(R.id.price_item),hasDescendant(withText("Standard Rate"))))
        .check(matches(withEffectiveVisibility(Visibility.VISIBLE)));
```
* Kakao

```Kotlin
onScreen<FormScreen> {
    price {
        isVisible()
    }
}
```

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Typed page objects

```Kotlin
class FormScreen : Screen<FormScreen>() {
    val phone = KEditText { withId(R.id.phone) }
    val email = KEditText { withId(R.id.email) }
    val submit = KButton { withId(R.id.submit) }
}
```

```Kotlin
onScreen<FormScreen> {
    phone {
        hasText("971201771")
    }
    submit {
        click()
    }
}
```

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Recycler support

```
class Item(parent: Matcher<View>) : KRecyclerItem<Item>(parent) {
    val title: KTextView = KTextView(parent) { withId(R.id.title) }
}
```

```Kotlin
val recycler: KRecyclerView = KRecyclerView(
  { withId(R.id.recycler_view) },
  itemTypeBuilder = { itemType(::Item) })
```
```
onScreen<RecyclerScreen> {
    recycler {
        firstChild<Item> {
            title { hasText("Title 1") }
       ...
```
---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Compose support

```Kotlin
class MainActivityScreen(semanticsProvider: SemanticsNodeInteractionsProvider) :
    ComposeScreen<MainActivityScreen>(
        semanticsProvider = semanticsProvider,
        viewBuilderAction = { hasTestTag("MainScreen") }
    ) {

    val myButton: KNode = child {
        hasTestTag("myTestButton")
    }

    val myButton2: KNode = myButton.child {
        hasTestTag("myTestButton2")
    }
}
```

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Compose support

```Kotlin
class ExampleInstrumentedTest {
    @Rule
    @JvmField
    val composeTestRule = createAndroidComposeRule<MainActivity>()

    @Test
    fun simpleTest() {
        onComposeScreen<MainActivityScreen>(composeTestRule) {
            myButton {
                assertIsDisplayed()
                assertTextContains("Button 1")
            }

            onNode { hasTestTag("doesNotExist") }.invoke {
                assertDoesNotExist()
            }
        }
    }
}

```
---
# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Quick refresh ...


`.../featureA/assesement.yaml`
```yaml
- case: 
  - id: featureAContentIsDisplayed
  - scenario:
  Open Feature A screen
  Wait for Content
  Verify that Loading is not dispalyed  
  Verify that Error is not dispalyed
  Verify that 3 items displayed in list
  Verify 1 item title is equals "Test title 1"  
  Verify 2 item title is equals "Test title 2"  
  Verify 3 item title is equals "Test title 3"
```

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Scenario Implementation

```Kotlin
@Test
fun featureAContentIsDisplayed()
onScreen<FeatureAScreen> {
    waitFor(R.id.title)
    loading { isNotDisplayed() }
    error { isNotDisplayed() }
    list { 
      isDisplayed() 
      childAt(0)<Item> { title { hasText("Test title 1") } }
      childAt(1)<Item> { title { hasText("Test title 2") } }
      childAt(2)<Item> { title { hasText("Test title 3") } }
    }
}
```

---

# ![2](https://icongr.am/material/numeric-2-circle.svg?color=666666) Kakao + Marathon = :revolving_hearts:

|Kakao|Marathon|
|---|---|
|:white_check_mark: Nice and simple DSL |:white_check_mark: Flakiness controll strategies|
|:white_check_mark: Typed page objects |:white_check_mark: Easy to scale|
|:white_check_mark: Recycler support|:white_check_mark: Can be integrated with CI|
|:white_check_mark: Compat API support|:white_check_mark: Supports custom configurations for your needs|
|:white_check_mark: Lots of Matcher and Assetions|:white_check_mark: Allure reporting|

---


# Agenda for today

- ![1](https://icongr.am/material/numeric-1-circle.svg?color=ff9900) Dive into UI testing 
- ![2](https://icongr.am/material/numeric-2-circle.svg?color=ff9900) Native Android UI Testing
- ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Mocks
- ![4](https://icongr.am/material/numeric-4-circle.svg?color=666666) Conclusion

---

<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/numeric-3-circle.svg?color=ff9900)

# Mocks

---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Mocks

* What are we testing?
  * Feature
  * Integration
* Why do we need mocks?
  * Performace improvement
  * Tests results stability
  * Control for 3rd party code

---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Mocks

* Deployment
  * Local
  * Remote
* Type
  * Stateless
  * Stateful
---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Testing with remote Prod server

|Pros|Cons|
|---|---|
|:white_check_mark: Testing whole HTTP stack|:x: Extra time for request-response processing|
|:white_check_mark: Good for Integration testing|:x: Bad for Feature testing|
||:x: Stateful|
||:x: Unstable results and branching in tests logic|

---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Testing with remote server (Mocks)

![w:900 center](images/call-1.svg)


---
# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Testing with remote server (Mocks)

|Pros|Cons|
|---|---|
|:white_check_mark: Testing whole HTTP stack|:x: Extra time for request-response processing|
|:white_check_mark: Single remote instance |:x: Required redeploy in case of mock updates|
|:white_check_mark: Stable results for UI testing |:x: Required redeploy in case of mock updates|
|:white_check_mark: Stateless |:x: Not supporting custom error codes|


---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Local Mocks (Entry level)

![w:900 center](images/call-2.svg)

---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Local Mocks (Entry level)

|Pros|Cons|
|---|---|
|:white_check_mark: Easy to implement|:x: HTTP Stack not tested|
|:white_check_mark: Fast responses |:x: Not shared between platforms|
|:white_check_mark: Static mock going as part of application ||
|:white_check_mark: Can be executed anywhere ||


---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Local Mocks (Intermediate)

* Custom ```TestRule```'s for custom behaviours
  * Response codes
  * Throttling
  * Responses alligned with Test Suits requrements
---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Local Mocks (Advanced)


![w:900 center](images/call-4.svg)

---

# ![3](https://icongr.am/material/numeric-3-circle.svg?color=666666) Local Mocks (Advanced)

|Pros|Cons|
|---|---|
|:white_check_mark: HTTP Stack tested |:x:  Required extra affort to implement|
|:white_check_mark: Mocks responses in-app |:x: Unsynchronized with "Production server"|
|:white_check_mark: Good for Features test|:x: Bad for Integration test|
|:white_check_mark: Can be run as "Remote server" ||
|:white_check_mark: Customizable behaviours ||



---

<!-- _class: lead -->

![1 w:256 h:256](https://icongr.am/material/numeric-4-circle.svg?color=ff9900)

# Conclusion

---


# ![3](https://icongr.am/material/numeric-4-circle.svg?color=666666) Conclusion

* Always undestand what you want to test
* Choose a right tooling set
* Ask yourself "How this code will change in 3 years"
* Think about scaling
* Questions?