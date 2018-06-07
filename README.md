# Angular Chrome Headless Docker
Docker image with embedded Node 10, Yarn and Chrome Headless preconfigured for Angular unit/e2e tests on your CI/CD servers.


### Get the image: 

`docker pull ngaxavi/angular-chrome-headless`

#### Launch scripts:

- unit tests:  `ng test --watch=false --browsers=ChromeHeadless`

- e2e tests:  `ng e2e`


### Karma Config:

```javascript

// Karma configuration file, see link for more information
// https://karma-runner.github.io/1.0/config/configuration-file.html

module.exports = function (config) {
  const configuration = {
    basePath: '',
    frameworks: ['jasmine', '@angular/cli'],
    browserNoActivityTimeout: 50000,
    browserDisconnectTolerance: 2,
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-jasmine-html-reporter'),
      require('karma-coverage-istanbul-reporter'),
      require('@angular/cli/plugins/karma')
    ],
    client:{
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    coverageIstanbulReporter: {
      reports: [ 'html', 'lcovonly' ],
      fixWebpackSourcePaths: true
    },
    angularCli: {
      environment: 'dev'
    },
    reporters: config.angularCli && config.angularCli.codeCoverage
      ? ['progress', 'coverage-istanbul']
      : ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['ChromeHeadless', 'Chrome'],
    customLaunchers: {
      ChromeHeadless: {
        base: 'Chrome',
        flags: [
          '--headless',
          '--disable-gpu',
          '--no-sandbox',
          '--remote-debugging-port=9222'
        ]
      }
    },
    singleRun: false
  };

  if (process.env.CI_SERVER) {
    configuration.browsers = ['ChromeHeadless'];
  }
  config.set(configuration);
};

```


