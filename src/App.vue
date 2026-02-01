<template>
  <div class="bg-rose-50 min-h-screen">
    <div class="grid grid-flow-row auto-rows-max gap-4 p-4">
      <div>
        <div class="flex justify-between">
          <div>
            <!-- previouse month  -->
          </div>
          <div>
            {{ monthYearString }}
          </div>
          <div>
            <!-- next month -->
          </div>
        </div>
      </div>
      <div>
        <hr />
      </div>
      <div>
        <div class="grid grid-cols-7 gap-4 text-center">
          <div
            v-for="dayNameAlias in ['Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa', 'Su']"
            :key="dayNameAlias"
          >
            {{ dayNameAlias }}
          </div>
          <div v-for="n in totalDaysOfMonth" :key="n">
            {{ n }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
console.log('👋 This message is being logged by "App.vue", included via Vite');

// Get current date reference
const today = new Date();

// Array of month names
const months = [
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
];

// Get the full year (4 digits)
const year = today.getFullYear(); // e.g., 2026

// Get the month index and access the name from the array
const monthString = months[today.getMonth()];

// Combine them into a single string
const monthYearString = `${monthString} ${year}`;

/**
 * Get the total number of days in a specific month
 * @param {number} monthOffset - Offset from current month (0 = last month, 1 = current, 2 = next)
 * @returns {number} Total days in the specified month
 */
const getTotalDaysInMonth = (monthOffset) => {
  return new Date(
    today.getFullYear(),
    today.getMonth() + monthOffset,
    0,
  ).getDate();
};

/**
 * Get the day of week for the first day of current month (Monday = 0, Sunday = 6)
 * @returns {number} Day of week adjusted for Monday as first day
 */
const getFirstDayOfMonth = () => {
  const firstDay = new Date(today.getFullYear(), today.getMonth(), 1).getDay();
  // Convert Sunday (0) to 6, and shift other days down by 1
  return firstDay === 0 ? 6 : firstDay - 1;
};

/**
 * Generate an array of day numbers for a given month
 * @param {number} totalDays - Number of days in the month
 * @returns {number[]} Array of day numbers [1, 2, 3, ...]
 */
const generateDayNumbers = (totalDays) => {
  return Array.from({ length: totalDays }, (_, index) => index + 1);
};

// Generate day numbers for each month
const lastMonthDays = generateDayNumbers(getTotalDaysInMonth(0));
const currentMonthDays = generateDayNumbers(getTotalDaysInMonth(1));
const nextMonthDays = generateDayNumbers(getTotalDaysInMonth(2));

// Build calendar grid (42 days = 6 weeks for complete calendar view)
const CALENDAR_GRID_SIZE = 42;
const daysFromLastMonth = getFirstDayOfMonth(); // Number of days from previous month to show

const totalDaysOfMonth = [
  ...lastMonthDays.slice(-daysFromLastMonth),
  ...currentMonthDays,
  ...nextMonthDays.slice(
    0,
    CALENDAR_GRID_SIZE - daysFromLastMonth - currentMonthDays.length,
  ),
];
</script>
