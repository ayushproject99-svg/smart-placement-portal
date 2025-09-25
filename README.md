# smart-placement-portal
// App.js
import React, { useState, useEffect } from 'react';

function App() {
  const [courses, setCourses] = useState([]);
  const [jobs, setJobs] = useState([]);
  const [selectedCourse, setSelectedCourse] = useState(null);

  useEffect(() => {
    // Fetch courses from backend API
    fetch('/api/courses')
      .then(res => res.json())
      .then(data => setCourses(data));
  }, []);

  useEffect(() => {
    if (selectedCourse) {
      fetch(`/api/jobs?course=${selectedCourse.id}`)
        .then(res => res.json())
        .then(data => setJobs(data));
    }
  }, [selectedCourse]);

  return (
    <div>
      <h1>Placement Portal</h1>
      <h2>Courses</h2>
      <ul>
        {courses.map(course => (
          <li key={course.id} onClick={() => setSelectedCourse(course)}>
            {course.name}
          </li>
        ))}
      </ul>

      {selectedCourse && (
        <>
          <h2>Jobs for {selectedCourse.name}</h2>
          <ul>
            {jobs.map(job => (
              <li key={job.id}>{job.title} - {job.company}</li>
            ))}
          </ul>
        </>
      )}
    </div>
  );
}

export default App;
