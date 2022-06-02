# working with git
    git clone git://git.drupalcode.org/projects/examples

# Here is the exercise:
    http://randyfay.com/node/72

# Learn to use git! Get logs:
    git log

# Get status:
    git status

# Add a file:  
    git add .
# Commit:
    git commit -m "test" file.txt
# Revert:
    git reset --hard HEAD^
# New branch:
    git checkout -b "render_example"
# See branch:
    git branch
# Add file to branch:
    git add .
# Compare to master:
    git diff master
# Return to master (switch):
    git checkout master

# You can also use TAB to autocomplete the command. 
    git --global user.name "your name"
    git --global user.email yours@exhf.com

# When you finish your work:
    git pull
    git push
