# A sample Guardfile
# More info at https://github.com/guard/guard#readme

guard 'rspec', :cli => '--tag ~slow', :version => 2 do
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^lib/patento/(.+)\.rb$})     { |m| "spec/#{m[1]}_spec.rb" }
  watch('spec/spec_helper.rb')  { "spec" }
end

