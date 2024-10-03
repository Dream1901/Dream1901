const express = require('express');
const bcrypt = require('bcrypt');
const mongoose = require('mongoose');

const app = express();
app.use(express.json());

mongoose.connect('mongodb://localhost:27017/cricketApp', { useNewUrlParser: true, useUnifiedTopology: true });

const UserSchema = new mongoose.Schema({
    username: String,
    password: String,
});

const User = mongoose.model('User', UserSchema);

// User Registration
app.post('/register', async (req, res) => {
    const { username, password } = req.body;
    const hashedPassword = await bcrypt.hash(password, 10);
    const newUser = new User({ username, password: hashedPassword });
    await newUser.save();
    res.status(201).send('User registered successfully');
});

// Start the server
app.listen(3000, () => {
    console.log('Server running on port 3000');
});
2. Player Selection (React Native Example)
javascript
Copy code
import React, { useState, useEffect } from 'react';
import { View, Text, Button, FlatList } from 'react-native';

const PlayerSelection = () => {
    const [players, setPlayers] = useState([]);

    useEffect(() => {
        fetch('https://api.example.com/players') // Replace with your API endpoint
            .then(response => response.json())
            .then(data => setPlayers(data));
    }, []);

    const selectPlayer = (player) => {
        // Logic to select player and update state
        console.log(player);
    };

    return (
        <View>
            <Text>Select Your Players:</Text>
            <FlatList
                data={players}
                renderItem={({ item }) => (
                    <View>
                        <Text>{item.name}</Text>
                        <Button title="Select" onPress={() => selectPlayer(item)} />
                    </View>
                )}
                keyExtractor={item => item.id}
            />
        </View>
    );
};

export default PlayerSelection;
3. Live Match Updates (Using WebSocket)
javascript
Copy code
const WebSocket = require('ws');

const ws = new WebSocket('wss://api.example.com/live-scores'); // Replace with your live scores WebSocket

ws.on('message', (data) => {
    const matchUpdate = JSON.parse(data);
    console.log('Match Update:', matchUpdate);
    // Logic to update app state with live scores
});
