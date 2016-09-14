browsers = [
    // windows 7, Chrome 41
    [ "os" : "Windows 7", "version": "41", "browser": "chrome", "deviceName": null, "deviceOrientation": null ],

    // windows xp, IE 8
    [ "os" : "Windows XP",  "version": "8", "browser": "internet explorer", "deviceName": null, "deviceOrientation": null],

    // windows 7, IE 9
    [ "os" : "Windows 7",  "version": "9", "browser": "internet explorer", "deviceName": null, "deviceOrientation": null],

    // windows 8, IE 10
    [ "os" : "Windows 8",  "version": "10", "browser": "internet explorer", "deviceName": null, "deviceOrientation": null],

    // windows 8.1, IE 11
    [ "os" : "Windows 8.1",  "version": "11", "browser": "internet explorer", "deviceName": null, "deviceOrientation": null],

    // OS X 10.8, Safari 6
    [ "os" : "OSX 10.8",  "version": "6", "browser": "safari", "deviceName": null, "deviceOrientation": null],

    // OS X 10.9, Safari 7
    [ "os" : "OSX 10.9",  "version": "7", "browser": "safari", "deviceName": null, "deviceOrientation": null],

    // OS X 10.10, Safari 7
    [ "os" : "OSX 10.10",  "version": "8", "browser": "safari", "deviceName": null, "deviceOrientation": null],

    // Linux, Firefox 37
    [ "os" : "Linux",  "version": "37", "browser": "firefox", "deviceName": null,"deviceOrientation":  null]
]

parallelTasks = [:]
for (int b = 0; b < browsers.toInteger(); b++) {
    browser = browser[i];
    branches["${browser.os} ${browser.browser} - ${browser.version}"] = {
        node {
            sauce('saucelabs') {
                sh "mvn test -d -DbrowserName=${browser.browser} -DbrowserOs=${browser.os} -DbrowserVersion=${browser.version}"
            }
        }
    }
}

node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'
    // Get some code from a GitHub repository
    // git url: 'https://github.com/saucelabs-sample-test-frameworks/Java-TestNG-Selenium.git'
    git url: 'https://github.com/halkeye/Java-Junit-Selenium.git'

    stage 'Compile'
    sh 'mvn compile'

    stage 'Test'
    parallel branches

    stage 'Collect Results'
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    step([$class: 'SauceOnDemandTestPublisher'])
}