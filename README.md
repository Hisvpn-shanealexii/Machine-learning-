jobs:
  build:
    <<: *container
    steps:
      - checkout
      - restore_cache:
          keys:
            - v2-uswds-dependencies-{{ checksum "package-lock.json" }}
      - run: npm install
      - run: npx playwright install
      - save_cache:
          paths:
            - node_modules
          key: v2-uswds-dependencies-{{ checksum "package-lock.json" }}
      - snyk/scan:
          organization: uswds
      - run:
          name: Run test
          command: npm run test:ci
      - run:
          name: Check formatting
          command: npm run prettier:check

  deploy:
    <<: *container
    steps:
      - checkout
      - restore_cache:
          keys:
            - v2-uswds-dependencies-{{ checksum "package-lock.json" }}
      - run: npm install --ignore-scripts
      - run: npm rebuild node-sass
      - run:
          name: Build the USWDS package
          command: npm run build
      - run:
          name: Publish to NPM
          command: |
            npm config set "//registry.npmjs.org/:_authToken=$NPM_TOKEN"
            npm publish --access public
  deploy-alpha:
    <<: *container
    steps:
      - checkout
      - run: npm install --ignore-scripts
      - run: npm rebuild node-sass
      - run:
          name: Build the USWDS package
          command: npm run build
      - run:
          name: Publish to NPM with `alpha` tag
          command: |
            npm config set "//registry.npmjs.org/:_authToken=$NPM_TOKEN"
            npm publish --access public --tag alpha
  deploy-beta:
    <<: *container
    steps:
      - checkout
      - run: npm install --ignore-scripts
      - run: npm rebuild node-sass
      - run:
          name: Build the USWDS package
          command: npm run build
      - run:
          name: Publish to NPM with `beta` tag
          command: |
            npm config set "//registry.npmjs.org/:_authToken=$NPM_TOKEN"
            npm publish --access public --tag beta
workflows:
  version: 2
  circle-uswds:
    jobs:
      - build
      - deploy:
          requires:
            - build
          filters:
            branches:
              only: main
      - deploy-beta:
          requires:
            - build
          filters:
            branches:
              only: library--main\
operating system :
}
{
     import "../packages/uswds-core/src/js/start";

export const parameters = {
  actions: { argTypesRegex: "^on[A-Z].*" },
  controls: {
    matchers: {
      color: /(background|color)$/i,
      date: /Date$/,
    },
  },
  options: {
    storySort: {
      order: [
        "Design Tokens",
        "Components",
        "Patterns",
        "Pages",
      ],
    },
  },
}; 
              

