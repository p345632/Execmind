// File: App.tsx
import React, { useState } from 'react';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';

export default function ExecMindApp() {
  const [task, setTask] = useState('');
  const [summary, setSummary] = useState('');
  const [meetingNotes, setMeetingNotes] = useState('');
  const [status, setStatus] = useState('');

  const handleSummarize = async () => {
    setStatus('Summarizing...');
    try {
      const response = await fetch('http://localhost:5000/summarize', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ meetingNotes }),
      });
      const data = await response.json();
      setSummary(data.summary);
      setStatus('Summary Ready');
    } catch (error) {
      setStatus('Error during summarization');
    }
  };

  return (
    <div className="p-6 max-w-3xl mx-auto">
      <h1 className="text-3xl font-bold mb-4">ExecMind - AI Chief of Staff</h1>

      {/* Meeting Summarizer */}
      <Card className="mb-4">
        <CardContent className="p-4">
          <h2 className="text-xl font-semibold mb-2">🧠 Meeting Summarizer</h2>
          <Textarea
            placeholder="Paste meeting transcript or notes here..."
            rows={6}
            value={meetingNotes}
            onChange={(e) => setMeetingNotes(e.target.value)}
          />
          <Button onClick={handleSummarize} className="mt-2">Summarize</Button>
          <p className="text-sm text-gray-500 mt-2">{status}</p>
          <div className="mt-4 p-2 bg-gray-100 rounded">
            <strong>Summary:</strong>
            <p>{summary}</p>
          </div>
        </CardContent>
      </Card>

      {/* Task Assistant */}
      <Card className="mb-4">
        <CardContent className="p-4">
          <h2 className="text-xl font-semibold mb-2">📌 Task Assistant</h2>
          <Input
            placeholder="What do you want to get done today?"
            value={task}
            onChange={(e) => setTask(e.target.value)}
          />
          <Button className="mt-2" onClick={() => alert(`Task Added: ${task}`)}>Add Task</Button>
        </CardContent>
      </Card>
    </div>
  );
}
