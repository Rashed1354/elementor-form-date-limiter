This is the code that I added on Elementor -> Custom code that allowed me to disable past days, Sundays, and 14-day limit. After you create the code, please make sure you select Location - - End and Always load jQuery is checked

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


<script>
    document.addEventListener('DOMContentLoaded', function() {
        // Function to disable Sundays
        function disableSundays() {
            var dateInput = document.getElementById('form-field-date_selector');
            if (!dateInput) return;

            // Ensure Flatpickr instance is available
            var calendarContainer = dateInput._flatpickr?.calendarContainer;
            if (!calendarContainer) return;

            calendarContainer.querySelectorAll('.flatpickr-day').forEach(function(day) {
                var date = new Date(day.getAttribute('aria-label'));
                if (date.getDay() === 0) { // 0 = Sunday
                    day.classList.add('disabled'); // Add 'disabled' class
                }
            });
        }
						// Try applying the class after Flatpickr initialization
        function attemptDisableSundays() {
            if (document.querySelector('.flatpickr-calendar')) {
                disableSundays();
            } else {
                setTimeout(attemptDisableSundays, 100); // Retry after a delay
            }
        }
        attemptDisableSundays();
    });
</script>
