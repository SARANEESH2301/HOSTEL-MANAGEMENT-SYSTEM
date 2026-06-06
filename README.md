# HOSTEL-MANAGEMENT-SYSTEM
A web-based hostel management solution developed using Python to streamline student registration, room allocation, attendance tracking, fee management, complaint handling, and administrative operations through an efficient and user-friendly interface.
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

# Database Configuration
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///hostel.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

# Student Model
class Student(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    reg_no = db.Column(db.String(20), unique=True, nullable=False)
    room_no = db.Column(db.String(10), nullable=False)

# Create Database
with app.app_context():
    db.create_all()

# Add Student
@app.route('/students', methods=['POST'])
def add_student():
    data = request.json

    student = Student(
        name=data['name'],
        reg_no=data['reg_no'],
        room_no=data['room_no']
    )

    db.session.add(student)
    db.session.commit()

    return jsonify({"message": "Student added successfully"}), 201

# Get All Students
@app.route('/students', methods=['GET'])
def get_students():
    students = Student.query.all()

    result = []
    for student in students:
        result.append({
            "id": student.id,
            "name": student.name,
            "reg_no": student.reg_no,
            "room_no": student.room_no
        })

    return jsonify(result)

# Run Application
if __name__ == '__main__':
    app.run(debug=True)
