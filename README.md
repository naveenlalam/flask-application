# flask-application

import React, { useState } from 'react';

const FileUpload = () => {
  const [file, setFile] = useState(null);
  const [response, setResponse] = useState(null);

  const handleFileChange = (event) => {
    setFile(event.target.files[0]);
  };

  const handleSubmit = async (event) => {
    event.preventDefault();

    try {
      const formData = new FormData();
      formData.append('file', file);

      // Replace 'YOUR_API_ENDPOINT' with the actual API URL
      const response = await fetch('YOUR_API_ENDPOINT', {
        method: 'POST',
        body: formData,
      });

      const data = await response.json();
      setResponse(data);
    } catch (error) {
      console.error('Error uploading file:', error);
    }
  };

  return (
    <div>
      <h1>File Upload</h1>
      <form onSubmit={handleSubmit}>
        <input type="file" onChange={handleFileChange} />
        <button type="submit">Upload</button>
      </form>

      {response && (
        <div>
          <h2>Response:</h2>
          <pre>{JSON.stringify(response, null, 2)}</pre>
        </div>
      )}
    </div>
  );
};

export default FileUpload;


import React from 'react';
import ExternalDataDisplay from './ExternalDataDisplay';

function App() {
  return (
    <div className="App">
      <ExternalDataDisplay />
    </div>
  );
}

export default App;
