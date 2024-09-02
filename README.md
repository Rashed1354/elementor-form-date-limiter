# Elementor Date Picker Custom Code

This repository contains custom JavaScript code for use in Elementor to manage date picker functionality. Specifically, it handles the following:

- Disabling past days.
- Disabling Sundays.
- Setting a 14-day date selection limit.

## Code Overview

### 1. Date Range Limitation and Past Days Disablement

The following script is added in Elementor under "Custom Code". It ensures that the date picker field cannot select past dates and limits the selectable range to 14 days from today.

```html
<script>
    document.addEventListener('DOMContentLoaded', function() {
        var today = new Date();
        var dd = String(today.getDate()).padStart(2, '0');
        var mm = String(today.getMonth() + 1).padStart(2, '0');
        var yyyy = today.getFullYear();
        var formattedToday = yyyy + '-' + mm + '-' + dd;

        var date_input = document.getElementById('form-field-date_selector'); // Correct ID

        if (date_input) {
            // Set today's date as the minimum date
            date_input.setAttribute("min", formattedToday);
            
            // Calculate the maximum date (14 days from today)
            var maxDate = new Date(today);
            maxDate.setDate(today.getDate() + 14);
            var maxDd = String(maxDate.getDate()).padStart(2, '0');
            var maxMm = String(maxDate.getMonth() + 1).padStart(2, '0');
            var maxYyyy = maxDate.getFullYear();
            var formattedMaxDate = maxYyyy + '-' + maxMm + '-' + maxDd;

            // Set the maximum date
            date_input.setAttribute("max", formattedMaxDate);

            // Ensure the value is within the allowed range
            date_input.addEventListener("input", function() {
                var selectedDate = new Date(this.value);
                if (selectedDate < today || selectedDate > maxDate) {
                    this.value = formattedToday;
                }
            });
        }
    });
</script>
