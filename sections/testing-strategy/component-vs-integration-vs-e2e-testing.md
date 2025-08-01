# Component vs (UI) Integration vs E2E tests

<br/>

### One Paragraph Explainer

Speaking about UI Testing (remember that we are speaking about the UI only, not the underlying JavaScript code), there are three main test types:
- **Component tests**: the unit tests of a UI, they test every single component in an isolated environment.

  Developing components in isolation is important because it allows you to isolate them from the corresponding container/use. A component exists to isolate a single behavior/content (the [Single responsibility principle](https://www.wikiwand.com/en/Single_responsibility_principle)) and therefore, coding it in isolation is profitable.

  There are many ways and tools to develop components in isolation but [Storybook](https://storybook.js.org) became the standard choice because of its effectiveness and its ecosystem. The latest Storybook versions wrap the excellent [Vitest Browser Mode](https://vitest.dev/guide/browser/), which is the fastest way to test UI components in isolation.

  Components have three types of contracts: the exposed data (**HTML**), their visual aspect (**CSS**), and the external APIs (**props and callbacks**). Testing every aspect could be cumbersome, that's where [Storybook](https://storybook.js.org/) comes in handy. It allows you to automate:
  - **the a11y (accessibility) tests**: HTML serves different purposes, SEO and a11y for example, and testing the accessibility of the HTML is the most thorough way to ensure HTML is semantically correct and maintain a high-level of compatibility with screen readers. As a result, SEO benefits too.
  - **the visual regression tests**: the visual aspect of the component compared **pixel by pixel** with the previous one, again, you are prompted to choose if you accept the changes or not.
  - **the interaction tests**: some interactions with a component expect correct state management. This kind of test must be written from the consumer point of view, not from the inner one (ex. the value of the input field when the user fills it, not the inner component state). An interaction/state test should assert the input field value after the keyboard events triggered. If the callbacks accepted by the component are correctly called is testable through this type of tests.


- <strong id="ui-integration-tests">UI integration tests</strong>: they run the whole app in a real browser **without hitting a real server**. These tests are the ace in the hole of every front-end developer. They are blazing fast and less exposed to random failures or false negatives.

  The front-end application does not know that there is not a real server: every AJAX call is resolved in no time by the testing tool. Static JSON responses (called "fixtures") are used to simulate the server responses. Fixtures allow us to test the front-end state simulating every possible back-end state.

  Another interesting effect: Fixtures **allow you to work without a working back-end** application. You can think about UI integration tests as "front-end-only tests".

  At the core of the most successful test suites, there is a lot of UI integration tests, considering the best type of test for your front-end app.

- **End-to-end (E2E) tests**: they run the whole app interacting with the real server. From the user interactions (one of the "end") to the business data (the other "end"): everything must work as designed. E2E tests are typically slow because
  - they need a **working back-end** application, typically launched alongside the front-end application. You can't launch them without a server, so you depend on the back-end developers to work
  - they need **reliable data**, seeding and cleaning it before every test

  That's why E2E tests are not feasible to be used as the only/main test type. They are pretty important because they are testing everything (front-end + back-end) but they must be used carefully to avoid brittle and hour-long test suites.

  In a complete suite with a lot of UI integration tests, you can think about E2E tests as "back-end tests". What flows should you test through them?
  - the Happy Path flows: you need to be sure that, at least, the users are able to complete the basic operations
  - everything valuable for your business: happy path or not, test whatever your business cares about (prioritizing them, obviously)
  - everything that breaks often: weak areas of the system must be monitored too

Identifying/defining the type of test is useful to group them, to [name the test files](/sections/generic-best-practices/name-test-files-wisely.md), to limit their scope, and to choose where to run them or not though the whole application and deployment pipelines.

<br /><br />

*Crossposted by [NoriSte](https://github.com/NoriSte) on [dev.to](https://dev.to/noriste/component-vs-ui-integration-vs-e2e-tests-3i0d) and [Medium](https://medium.com/@NoriSte/component-vs-ui-integration-vs-e2e-tests-f02b575339dc).*
