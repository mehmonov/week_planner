# Weekly Task Manager

A Vue.js-based weekly task management application with a clean, modern interface and dark mode support. This application helps users organize their weekly tasks effectively with day-specific task management and progress tracking.

## Features

The Weekly Task Manager comes with a comprehensive set of features designed to enhance task management:

### Task Management
- Day-specific task creation and organization
- Intuitive task completion tracking
- Easy task deletion with hover controls
- Smooth animations for task interactions

### Progress Tracking
- Real-time completion statistics
- Weekly progress visualization
- Days remaining counter
- Current week number display

### User Interface
- Responsive design that works on all device sizes
- Dark mode support
- Clean, modern interface with gradient backgrounds
- Smooth transitions and animations

### Data Persistence
- Automatic saving of tasks to localStorage
- Task persistence across browser sessions

## Technical Stack

- Vue.js 3 (with Composition API)
- Tailwind CSS for styling
- Lucide Icons for UI elements
- Local Storage for data persistence

## Installation

1. Clone the repository:
```bash
git clone https://github.com/mehmonov/weekly-planner.git
```

2. Navigate to the project directory:
```bash
cd weekly-planner
```

3. Install dependencies:
```bash
npm install
```

4. Start the development server:
```bash
npm run dev
```

## Usage

### Adding Tasks

1. Select the day for your task from the dropdown menu
2. Enter your task in the input field
3. Click "Add Task" or press Enter

### Managing Tasks

- Mark tasks as complete by clicking the checkbox
- Delete tasks by hovering over them and clicking the trash icon
- Tasks are automatically saved as you make changes

### Viewing Progress

The top section displays three key metrics:
- Tasks Completed: Shows total completion count and progress bar
- Weekly Progress: Displays completion percentage and days remaining
- Current Week: Shows the current week number and date

## Project Structure

```
weekly-task-manager/
├── src/
│   ├── components/
│   │   └── WeeklyTaskManager.vue
│   ├── App.vue
│   └── main.js
├── public/
├── package.json
└── README.md
```

## Customization

### Styling

The project uses Tailwind CSS for styling. You can customize the appearance by:

1. Modifying the color scheme in the gradient classes:
```html
<div class="bg-gradient-to-br from-teal-50 to-blue-50 dark:from-gray-900 dark:to-gray-800">
```

2. Adjusting the card styles:
```html
<div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-sm">
```

### Adding New Features

The component-based architecture makes it easy to add new features:

1. Task filtering
2. Task categories
3. Task priority levels
4. Due dates
5. Task notes or descriptions

## Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin feature/my-new-feature`
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Vue.js team for the excellent framework
- Tailwind CSS for the utility-first CSS framework
- Lucide Icons for the beautiful icons

## Support

For support, please open an issue in the GitHub repository or contact the maintainers.

