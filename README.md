# Cypress

[Testing | Next.js](https://nextjs.org/docs/testing#cypress)

> `Cypress`는 **End-to-End(E2E)**와 **Integration Testing**위해 사용하는 test runner입니다.
>

**Quickstart** 또는 **Manual setup** 중 하나를 사용하여 `Cypress`를 시작합니다.

## Quickstart

[next.js/examples/with-cypress at canary · vercel/next.js](https://github.com/vercel/next.js/tree/canary/examples/with-cypress)

```jsx
$ npx create-next-app@latest --example with-cypress with-cypress-app
```

## Manual setup

```bash
# intall the cypress package
$ npm install --save-dev cypress
```

Add Cypress the `package.json` script field:

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "cypress": "cypress open",
}
```

Run Cypress for the first time:

```json
$ npm run cypress
```

Cypress 공식 문서

[Writing Your First Test | Cypress Documentation](https://docs.cypress.io/guides/getting-started/writing-your-first-test)

### Cypress integration test

만들어진 Next + Cypress 프로젝트에서 `cypress/integration/app.spec.js`를 확인합니다.

Next Start 페이지를 기반한 Cypress testing 코드가 작성되어 있습니다. 코드를 간단히 살펴 보면...

```jsx
// cypress/integration/app.spec.js

describe('Navigation', () => {
  it('should navigate to the about page', () => {
    // 1
    cy.visit('http://localhost:3000/')

    // 2
    cy.get('a[href*="about"]').click()

    // 3
    cy.url().should('include', '/about')

    // 4
    cy.get('h1').contains('About Page')
  })
})
```

이 테스트는 Navigation 테스트 이며 'about 페이지로 이동해야한다'라고 합니다.  그리고 아래 동작을 확인합니다.

1. index 페이지 시작 시 [`http://localhost:3000/`를](http://localhost:3000/를) 방문해야 합니다.
2. a 태그 중 href 속성이 about인 요소를 찾아 클릭합니다.
3. 새 url은 `/about`를 포함해야 합니다.
4. 새 페이지의 h1 태그에는 "About Page"를 포함해야 합니다.

코드를 간단히 살펴보았습니다. 그렇다면 직접 cypress를 통해서 testing을 진행해 볼텐데요. package.json에서 아래와 같은 설정을 확인할 수 있습니다.

```json
...
"scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "cypress": "cypress open",
    "cypress:headless": "cypress run",
    "e2e": "start-server-and-test dev http://localhost:3000 cypress",
    "e2e:headless": "start-server-and-test dev http://localhost:3000 cypress:headless"
},
...
```

`Cypress`를 통해 테스트를 진행하기 위해서는 프로젝트를 **빌드**하고 **실행**해야 합니다.

```bash
$ npm run build
$ npm run start

# another terminal
$ npm run cypress
```

**build를 통해 start를 하고 나면 cypress는 또 다른 터미널에서 실행합니다.**

** 지속적으로 개발된 프로젝트에서는 매번 빌드 이후에 cypress testing을 진행해야 정상적으로 testing이 가능합니다.

+) 추가적으로 cypress.json에서 `"baseUrl": "http://localhost:3000"`을 추가하면 `cy.visit("http:localhost:3000")` 대신 `cy.visit("/")`를 사용할 수 있습니다.
