task :deploy do
  sh "jekyll build"
  sh "git subtree push --prefix _site origin gh-pages"
end
