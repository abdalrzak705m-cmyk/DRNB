const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server, {
    cors: { origin: "*" }
});

app.get('/', (req, res) => {
    res.send('Secret Chat Server is running successfully!');
});

io.on('connection', (socket) => {
    console.log(`مستخدم متصل برقم تعريف الاتصال: ${socket.id}`);

    socket.on('register_user', (userId) => {
        socket.join(userId);
        console.log(`تم تسجيل المستخدم في غرفته الخاصة: ${userId}`);
    });

    socket.on('send_standard_message', (data) => {
        io.to(data.receiverId).emit('receive_standard_message', data);
    });

    socket.on('send_secret_message', (data) => {
        io.to(data.receiverId).emit('receive_secret_message', data);
    });

    socket.on('join_group', (groupId) => {
        socket.join(groupId);
        console.log(`مستخدم انضم للمجموعة: ${groupId}`);
    });

    socket.on('send_group_message', (data) => {
        io.to(data.groupId).emit('receive_group_message', data);
    });

    socket.on('disconnect', () => {
        console.log('مستخدم انقطع عن الاتصال');
    });
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
