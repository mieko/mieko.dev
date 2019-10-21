task :deploy do
  sh 'jekyll build'
  sh 'touch _site/.nojekyll'
  ENV['GIT_DEPLOY_DIR'] = '_site'
  ENV['GIT_DEPLOY_BRANCH'] = 'gh-pages'
end
