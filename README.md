package com.cts.library.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.validation.constraints.Min;
import jakarta.validation.constraints.NotBlank;
import lombok.Data;

@Entity
@Data
public class Book {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long bookId;
	@NotBlank(message = "Book Name is required")
	private String bookName;
	@NotBlank(message = "Book Author is required")
	private String author;
	@NotBlank(message = "Book Genre is required")
	private String genre;
	@NotBlank(message = "Book ISBN is required")
	private String ISBN;
	@Min(value = 1000, message = "Invalid Publication year")
	private int yearPublished;
	@Min(value = 0, message = "Available copies cannot be negative")
	private int availableCopies;

	public Book() {
		super();
	}

	public Book(long bookId, @NotBlank(message = "Book Name is required") String bookName,
			@NotBlank(message = "Book Author is required") String author,
			@NotBlank(message = "Book Genre is required") String genre,
			@NotBlank(message = "Book ISBN is required") String iSBN,
			@Min(value = 1000, message = "Invalid Publication year") int yearPublished,
			@Min(value = 0, message = "Available copies cannot be negative") int availableCopies) {
		super();
		this.bookId = bookId;
		this.bookName = bookName;
		this.author = author;
		this.genre = genre;
		ISBN = iSBN;
		this.yearPublished = yearPublished;
		this.availableCopies = availableCopies;
	}

	public long getBookId() {
		return bookId;
	}

	public void setBookId(long bookId) {
		this.bookId = bookId;
	}

	public String getBookName() {
		return bookName;
	}

	public void setBookName(String bookName) {
		this.bookName = bookName;
	}

	public String getAuthor() {
		return author;
	}

	public void setAuthor(String author) {
		this.author = author;
	}

	public String getGenre() {
		return genre;
	}

	public void setGenre(String genre) {
		this.genre = genre;
	}

	public String getISBN() {
		return ISBN;
	}

	public void setISBN(String iSBN) {
		ISBN = iSBN;
	}

	public int getYearPublished() {
		return yearPublished;
	}

	public void setYearPublished(int yearPublished) {
		this.yearPublished = yearPublished;
	}

	public int getAvailableCopies() {
		return availableCopies;
	}

	public void setAvailableCopies(int availableCopies) {
		this.availableCopies = availableCopies;
	}

	public boolean isAvailable() {
		return availableCopies > 0;
	}

}

package com.cts.library.model;

import jakarta.persistence.*;
import java.time.LocalDate;

import com.fasterxml.jackson.annotation.JsonBackReference;

@Entity
public class BorrowingTransaction {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long transactionId;

	@ManyToOne
	@JoinColumn(name = "bookId", nullable = false)
	private Book book;

	@ManyToOne
	@JoinColumn(name = "memberId", nullable = false)
	@JsonBackReference
	private Member member;

	private LocalDate borrowDate;
	private LocalDate returnDate;

	@Enumerated(EnumType.STRING)
	private Status status;

	public enum Status {
		BORROWED, RETURNED
	}

	// Constructors
	public BorrowingTransaction() {
	}

	public BorrowingTransaction(Book book, Member member, LocalDate borrowDate, LocalDate returnDate, Status status) {
		this.book = book;
		this.member = member;
		this.borrowDate = borrowDate;
		this.returnDate = returnDate;
		this.status = status;
	}

	// Getters and Setters

	public Long getTransactionId() {
		return transactionId;
	}

	public void setTransactionId(Long transactionId) {
		this.transactionId = transactionId;
	}

	public Book getBook() {
		return book;
	}

	public void setBook(Book book) {
		this.book = book;
	}

	public Member getMember() {
		return member;
	}

	public void setMember(Member member) {
		this.member = member;
	}

	public LocalDate getBorrowDate() {
		return borrowDate;
	}

	public void setBorrowDate(LocalDate borrowDate) {
		this.borrowDate = borrowDate;
	}

	public LocalDate getReturnDate() {
		return returnDate;
	}

	public void setReturnDate(LocalDate returnDate) {
		this.returnDate = returnDate;
	}

	public Status getStatus() {
		return status;
	}

	public void setStatus(Status status) {
		this.status = status;
	}
}

package com.cts.library.model;

import java.time.LocalDate;

import com.fasterxml.jackson.annotation.JsonBackReference;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;

@Entity
public class Fine {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long fineId;

    @ManyToOne
    @JoinColumn(name = "member_id", nullable = false)
    @JsonBackReference
    private Member member;

    @ManyToOne
    @JoinColumn(name = "transaction_id", nullable = false)
    private BorrowingTransaction transaction;

    private double amount;
    private String fineStatus;
    private LocalDate transactionDate;

    public Fine() {
    }

    public Fine(Long fineId, Member member, double amount, String fineStatus, LocalDate transactionDate) {
        this.fineId = fineId;
        this.member = member;
        this.amount = amount;
        this.fineStatus = fineStatus;
        this.transactionDate = transactionDate;
    }

    public Long getFineId() {
        return fineId;
    }

    public void setFineId(Long fineId) {
        this.fineId = fineId;
    }

    public Member getMember() {
        return member;
    }

    public void setMember(Member member) {
        this.member = member;
    }

    public BorrowingTransaction getTransaction() {
        return transaction;
    }

    public void setTransaction(BorrowingTransaction transaction) {
        this.transaction = transaction;
    }

    public double getAmount() {
        return amount;
    }

    public void setAmount(double amount) {
        this.amount = amount;
    }

    public String getFineStatus() {
        return fineStatus;
    }

    public void setFineStatus(String fineStatus) {
        this.fineStatus = fineStatus;
    }

    public LocalDate getTransactionDate() {
        return transactionDate;
    }

    public void setTransactionDate(LocalDate transactionDate) {
        this.transactionDate = transactionDate;
    }
}

package com.cts.library.model;

public class LoginDetails {

	private String userName;
	private String userPassword;
	private Role role;
	
	public Role getRole() {
		return role;
	}
	public void setRole(Role role) {
		this.role = role;
	}
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	public String getUserPassword() {
		return userPassword;
	}
	public void setUserPassword(String userPassword) {
		this.userPassword = userPassword;
	}
}









package com.cts.library.model;

import java.time.LocalDate;
import java.util.List;

import com.fasterxml.jackson.annotation.JsonManagedReference;
import com.fasterxml.jackson.annotation.JsonProperty;

import jakarta.persistence.CascadeType;

import jakarta.persistence.Entity;
import jakarta.persistence.EnumType;
import jakarta.persistence.Enumerated;

import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;
import lombok.Data;

@Entity
@Data
public class Member {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long memberId;

	@NotBlank(message = "Name cannot be Blank")
	private String name;

	@NotBlank(message = "Email cannot be Blank")
	@Email(message = "Invalid email format")
	private String email;

	@NotBlank(message = "Phone Number cannot be Blank")
	@Size(min = 10, max = 10, message = "Phone Number has to be 10 digits only")
	private String phone;

	@NotBlank(message = "Address cannot be Blank")
	private String address;

	@NotBlank(message = "UserName cannot be Blank")
	@Size(min = 4, max = 20, message = "UserName must be between 4 to 20 characters")
	private String username;

	@NotBlank(message = "Password cannot be Blank")
	@Size(min = 8, message = "Password must be between 8 to 50 characters")
	@JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
	private String password;

	private LocalDate membershipExpiryDate;
	private int borrowingLimit = 2;

	@Enumerated(EnumType.STRING)
	private MembershipStatus membershipStatus = MembershipStatus.BASIC;

	@Enumerated(EnumType.STRING)
	private Role role;

	@OneToMany(mappedBy = "member", cascade = CascadeType.ALL, orphanRemoval = true)
	@JsonManagedReference
	private List<BorrowingTransaction> transactions;

	@OneToMany(mappedBy = "member", cascade = CascadeType.ALL, orphanRemoval = true)
	@JsonManagedReference
	private List<Fine> fines;

	@OneToMany(mappedBy = "member", cascade = CascadeType.ALL, orphanRemoval = true)
	@JsonManagedReference
	private List<Notification> notifications;

	public List<BorrowingTransaction> getTransactions() {
		return transactions;
	}

	public void setTransactions(List<BorrowingTransaction> transactions) {
		this.transactions = transactions;
	}

	public List<Fine> getFines() {
		return fines;
	}

	public void setFines(List<Fine> fines) {
		this.fines = fines;
	}

	public boolean isMembershipActive() {
		return membershipExpiryDate != null && LocalDate.now().isBefore(membershipExpiryDate);
	}

	public Member() {
		// by default we are setting the role to member
		this.role = Role.MEMBER;
	}

	public Member(long memberId, String name, String email, String phone, String address,
			MembershipStatus membershipStatus, String username, String password, Role role) {
		this(memberId, name, email, phone, address, membershipStatus, 2, username, password, role);
	}

	public Member(long memberId, String name, String email, String phone, String address,
			MembershipStatus membershipStatus, int borrowingLimit, String username, String password, Role role) {
		super();
		this.memberId = memberId;
		this.name = name;
		this.email = email;
		this.phone = phone;
		this.address = address;
		this.membershipStatus = membershipStatus;
		this.username = username;
		this.password = password;
		this.role = role;
		this.borrowingLimit = borrowingLimit;
	}

	public long getMemberId() {
		return memberId;
	}

	public void setMemberId(long memberId) {
		this.memberId = memberId;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public MembershipStatus getMembershipStatus() {
		if (membershipExpiryDate == null || LocalDate.now().isAfter(membershipExpiryDate)) {
			return MembershipStatus.EXPIRED;
		}
		return membershipStatus;
	}

	public void setMembershipStatus(MembershipStatus status) {
		if (this.membershipStatus != MembershipStatus.PRIME || status == MembershipStatus.EXPIRED) {
			this.membershipStatus = status;
		}
	}

	public LocalDate getMembershipExpiryDate() {
		return membershipExpiryDate;
	}

	public void setMembershipExpiryDate(LocalDate membershipExpiryDate) {
		this.membershipExpiryDate = membershipExpiryDate;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public Role getRole() {
		return role;
	}

	public void setRole(Role role) {

		// we are making sure member is not changing into admin
		if (this.role != Role.ADMIN) {
			this.role = role;
		}
	}

	public int getBorrowingLimit() {
		return borrowingLimit;
	}

	public void setBorrowingLimit(int borrowingLimit) {
		this.borrowingLimit = borrowingLimit;
	}

}
package com.cts.library.model;

public enum MembershipStatus {
		
	BASIC,  // this is free tier plan
	PRIME,  // this is premium membership plan
	EXPIRED // plan is expired
}

package com.cts.library.model;

import java.time.LocalDateTime;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.OneToOne;

@Entity
public class MemberToken {
	
	@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long tokenId;
 
    @Column(unique = true)
    private String memberToken;
 
    private LocalDateTime generatedAt;
 
    @OneToOne
    @JoinColumn(name = "memberId")
    private Member member;
    
    public MemberToken() {
    	
    }
 
	public Long getTokenId() {
		return tokenId;
	}
 
	public void setTokenId(Long tokenId) {
		this.tokenId = tokenId;
	}
 
	public String getMemberToken() {
		return memberToken;
	}
 
	public void setMemberToken(String memberToken) {
		this.memberToken = memberToken;
	}
 
	public LocalDateTime getGeneratedAt() {
		return generatedAt;
	}
 
	public void setGeneratedAt(LocalDateTime generatedAt) {
		this.generatedAt = generatedAt;
	}
 
	public Member getMember() {
		return member;
	}
 
	public void setMember(Member member) {
		this.member = member;
	}

}
package com.cts.library.model;

import jakarta.persistence.*;
import java.util.Date;

import com.fasterxml.jackson.annotation.JsonBackReference;

@Entity
@Table(name = "notifications")
public class Notification {
	
	@Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long notificationId;

    private String message;

    private Date dateSent;

    @ManyToOne
    @JoinColumn(name="member_id")
    @JsonBackReference
    private Member member;

    @ManyToOne
    @JoinColumn(name="book_id")
    private Book book;

    @ManyToOne
    @JoinColumn(name="fine_id")
    private Fine fine;

    private long overdueDays;

    public Notification() {}

    public Notification(long overdueDays, Member member, Book book, Fine fine, String message, Date dateSent) {
        this.overdueDays = overdueDays;
        this.member = member;
        this.book = book;
        this.fine = fine;
        this.message = message;
        this.dateSent = dateSent;
    }
	
	public Long getNotificationId() {
		return notificationId;
	}

	public void setNotificationId(Long notificationId) {
		this.notificationId = notificationId;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}

	public Date getDateSent() {
		return dateSent;
	}

	public void setDateSent(Date dateSent) {
		this.dateSent = dateSent;
	}

	public Member getMember() {
		return member;
	}

	public void setMember(Member member) {
		this.member = member;
	}

	public Book getBook() {
		return book;
	}

	public void setBook(Book book) {
		this.book = book;
	}

	public Fine getFine() {
		return fine;
	}

	public void setFine(Fine fine) {
		this.fine = fine;
	}

	public long getOverdueDays() {
		return overdueDays;
	}

	public void setOverdueDays(long overdueDays) {
		this.overdueDays = overdueDays;
	}

	
}
package com.cts.library.model;

public enum Role {
	ADMIN,
	MEMBER
}
