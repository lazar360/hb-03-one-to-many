# hb-03-one-to-many

@Entity
@Table(name ="course")
public class Course {
	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="title")
	private String title;
	
	@ManyToOne(cascade = {CascadeType.DETACH, CascadeType.MERGE, 
			CascadeType.PERSIST, CascadeType.REFRESH})
	@JoinColumn(name="instructor_id")
	private Instructor instructor;

...}

@Entity
@Table(name="instructor")
public class Instructor {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
	
	@OneToOne(cascade = CascadeType.ALL)
	@JoinColumn(name="instructor_detail_id") //
	private InstructorDetail instructorDetail;

	// le mappedBy renvoie toujours à une classe java et, comme c'est many, on relie instructor à une liste de cours
	@OneToMany(mappedBy = "instructor", cascade = {CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH})
	private List<Course> courses;

...

// add convenience method for bi-directional
	public void add(Course tempCourse) {
		if (courses == null) {
			courses = new ArrayList<>();
		}
		
		courses.add(tempCourse);
		
		tempCourse.setInstructor(this);
	}
