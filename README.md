# flask-application


import React, { useState, useEffect } from 'react';
import axios from 'axios'; // Only if you choose to use Axios

const ExternalDataDisplay = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    // Function to fetch data from the API
    const fetchData = async () => {
      try {
        // Replace 'YOUR_API_ENDPOINT' with the actual API URL
        const response = await fetch('YOUR_API_ENDPOINT');
        const data = await response.json();
        setData(data);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    fetchData(); // Call the function to fetch data when the component mounts
  }, []);

  return (
    <div>
      <h1>External API Data:</h1>
      {data.map((item) => (
        <div key={item.id}>
          <h3>{item.name}</h3>
          {/* Display other data fields as needed */}
        </div>
      ))}
    </div>
  );
};

export default ExternalDataDisplay;
