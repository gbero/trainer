# trainer

This is an alternative approach to generate JUnit files for your CI (e.g. Jenkins) without parsing the `xcodebuild` output, but using the Xcode `plist` files instead.

The new Xcode beta has a known issue around not properly closing `stdout` ([Radar](https://openradar.appspot.com/27447948)), so you [can't use xcpretty](https://github.com/supermarin/xcpretty/issues/227).

`trainer` is a more robust and faster approach to generate JUnit reports for your CI system. 

> By using `trainer`, the Twitter iOS code base now generates JUnit reports 10 times faster.

## Use with [fastlane](https://fastlane.tools)

Update to the latest [fastlane](https://fastlane.tools) and run

```bash
fastlane add_plugin trainer
```

Now add the following to your `Fastfile`

```ruby
lane :test do
  scan(workspace: "MyApp.xcworkspace",
       output_types: "",
       derived_data_path: "/tmp/fastlane_trainer/#{Time.now.to_i}")
  trainer
end
```

This will generate the JUnit file in the temporary location `/tmp/fastlane_trainer/[time]`. You can specify any path you want, just make sure to have it clean for every run so that your CI system knows which one to pick.

For more information, check out the [fastlane plugin docs](fastlane-plugin-trainer#readme).

## Without [fastlane](https://fastlane.tools)

### Installation

Add this to your `Gemfile` 
```
gem trainer
```
and run
```
bundle install
```

Alternatively you can install the gem system-wide using `sudo gem install trainer`.

### Usage

If you use `fastlane`, check out the official [fastlane plugin](fastlane-plugin-trainer#readme) on how to use `trainer` in `fastlane`.

#### Run tests

```
cd [project]
scan --derived_data_path "output"
```

#### Convert the plist files to junit

```
trainer
```

You can also pass a custom directory containing the plist files

```
trainer --path ./something
```

For more information run

```
trainer --help
````

### Show the test results right in your pull request

To make it easier for you and your contributors to see the test failures, you should start using [danger](http://danger.systems) with the [danger-junit](https://github.com/orta/danger-junit) plugin

![](https://raw.githubusercontent.com/orta/danger-junit/master/img/example.png)

### Thanks

After the [lobbying of @steipete](https://twitter.com/steipete/status/753662170848690176) and the comment

> How does Xcode Server parse the results?

I started investigating alternative approaches on how to parse test results.
