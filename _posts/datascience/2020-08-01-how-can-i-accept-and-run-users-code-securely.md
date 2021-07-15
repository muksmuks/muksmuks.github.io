---
layout: post
title:  "How can i accept and run user's code securely on my web app?"
date:   2021-07-15 20:02:00 +0700
categories: [python, django, security]
---





Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
Cras facilisis, ligula id sodales dignissim, ligula felis maximus metus, 
nec malesuada odio ipsum ut mi. 
<!--excerpt -->

Quisque suscipit condimentum nisi sed ornare. 
Ut dictum id massa et finibus. 
Donec ut consequat massa. Aenean eget mauris quam. Sed ornare auctor sapien fringilla condimentum. 

<div class="excerpt">
    <h3>Recipe</h3>
    {{ page.excerpt }}
</div>
          </row>
  </div>
  </div>
  <div class="col-lg-6 col-sm-12 right split-cols">
      <div class="content">
          <header class="major">
              <h2>{{ page.title }}</h2>
          </header>
          {{content|remove_first: page.excerpt}}
      </div>


