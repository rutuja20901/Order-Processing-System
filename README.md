# Order-Processing-System
hi



StudentController
package com.cache.implement.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import com.cache.implement.entity.Student;
import com.cache.implement.service.StudentService;

@RestController
public class StudentController {
	
	@Autowired
	private StudentService studentService;
	
	@PostMapping("/add")
	public Student addStudent(@RequestBody Student student) {
		System.out.println("controller working properly......");
		return studentService.registerStudent(student);
		
	}
	
	
	@GetMapping("/list")
	public List<Student> listStudent(){
		return studentService.listStudent();
	}
	

}


Student


package com.cache.implement.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;

@Entity
@Table(name="stud")
public class Student {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;
	
	private String name;
	
	private String course;

	public Student() {
		super();
	}

	public Student(String name, String course) {
		super();
		this.name = name;
		this.course = course;
	}

	public Student(long id, String name, String course) {
		super();
		this.id = id;
		this.name = name;
		this.course = course;
	}

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getCourse() {
		return course;
	}

	public void setCourse(String course) {
		this.course = course;
	}

	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", course=" + course + "]";
	}
	
	

}



StudentRepository


package com.cache.implement.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.cache.implement.entity.Student;

@Repository
public interface StudentRepository extends JpaRepository<Student, Long>{

	
}


StudentService


package com.cache.implement.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

import com.cache.implement.entity.Student;
import com.cache.implement.repository.StudentRepository;



@Service
public class StudentService {

	@Autowired
	private StudentRepository studentRepository;

	/*
	 Cacheable - Cache is only implement at GET service 
	 CacheEvict - Use on ADD/UPDATE/DELETE 
	 */
	
	
	@Cacheable(value="studentList")
	public List<Student> listStudent() {
		System.out.println("Fetching from database");
		return studentRepository.findAll();

	}
	
	@CacheEvict(value="studentList",allEntries=true)
	public Student registerStudent(Student student) {
	
		return studentRepository.save(student);
	}

	
	

}
