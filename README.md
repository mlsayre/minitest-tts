minitest-tts
============

Step-by-step instructions to get spoken alerts into your minitests.

1) Make sure the minitest gem is installed. 

- These instructions assume minitest v.4.7.5. 
- Write some tests for minitest. Using RSpec style here from RailsCast #327 “MiniTests with Rails” project.


2) Install speaker gem.

- Based on Google Translator API.
- gem 'speaker' in :test group (using v.0.0.4 here) then $ bundle install
- Creates audio.mp3 file in project’s main directory. No need to add to GitHub.
- To play mp3 file, Macs use afplay. May already be installed. Ubuntu uses mpg123. Need to install that if you use Ubuntu.         $ sudo apt-get install mpg123


3)  To receive alert when a specific test is about to run:

- In the test file, add         Speaker.new(text: "Your test is about to run. Come hither!!").tts  
directly before test is going to be run. Example:

            describe ProductsHelper do
              it "converts number to currency" do
                Speaker.new(text: "converts number to currency").tts
                number_to_currency(5).must_equal "$5.00"
              end

4) To receive alerts after each test has completed:

- Modify minitest/unit.rb file.
- Must use sudo in order to modify the file: $ sudo subl unit.rb
- Add speaker code directly into minitest/unit.rb file, around line 1219 in the after_teardown hook:

             def after_teardown;
               Speaker.new(text: "one test complete").tts
             end
     
5) To receive alert when all the tests have completed: 

- Add speaker code directly into the minitest/unit.rb file (sudo again), around line 891:

            puts "Finished #{type}s in %.6fs, %.4f tests/s, %.4f assertions/s." %
              [t, test_count / t, assertion_count / t]
      
            Speaker.new(text: "Your tests are done. Stop playing xbox.").tts
      
            report.each_with_index do |msg, i|
              puts "\n%3d) %s" % [i + 1, msg]
            end

6) Done! Rake and enjoy the ability to stretch your legs and know exactly when your tests have been completed.

TODO : 
- Notify you when a test passes or fails. 
- Speak the number of test failures, errors, or passes when all tests have completed. 
- Figure out how to turn this into a gem so it’s easier to install and use. Name it minitesttts to assure many typos.

