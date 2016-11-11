# Webflow Tricks for Developers

- Turn off 'improperly configured forms' error:
  - Error:
    Oops! This page has improperly configured forms. Please contact your website administrator to fix this issue.
  - Solution:
    webflow.js#1164:
    ```
    function init() {
      return; // <---------- add this line
      siteId = $('html').attr('data-wf-site');

      $forms = $(namespace + ' form');
      if (!$forms.length) return;
      $forms.each(build);
    }
    ```

- Fix nested drowdown close error:
  - Error:
    When you click an item in a nested dropdown, the outer dropdown closes
  - Solution:
    - webflow.js#951
      ```
      function outside(data) {
        // Unbind previous tap handler if it exists
        if (data.outside) $doc.off('tap' + namespace, data.outside);

        // Close menu when tapped outside
        return _.debounce(function(evt) {
          if (!data.open) return;
          var $target = $(evt.target);
          if ($target.closest('.w-dropdown-toggle').length) return;
          if ($target.hasClass('w-keep-dropdown-open')) return; <---------- add this line
          if (!data.el.is($target.closest(namespace))) {
            close(data);
          }
        });
      }
      ```
    - on all elements that you do not want to trigger drowdown close, add the
      class 'w-keep-dropdown-open'

- Make animation and javascript work:
  - Solution:
    run the following when the dom is ready:
    ```
    window.Webflow.destroy();
    window.Webflow.ready();
    ```
