Building a Registration Window in Kotlin with Jetpack Compose Creating a modern, clean registration window is one of the most common tasks in Android development. By leveraging Kotlin and Jetpack Compose, you can build a responsive, secure, and beautiful UI with minimal boilerplate code.

Here is a step-by-step breakdown of how a standard registration screen is structured.

  Managing the State Before building the UI, we need to track what the user types. In Compose, we use mutableStateOf to handle the inputs for the username, email, password, and confirm password fields.

Crafting the UI Components A standard registration form consists of:

OutlinedTextFields: For user input, equipped with clear labels and icons.

VisualTransformation: To hide the password characters for security.

Button: To trigger the registration logic.

EXAMPLE 1
```
Menu Optinons 1

package com.example.menuoptionsv2

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.padding
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.AccountCircle
import androidx.compose.material.icons.filled.Email
import androidx.compose.material.icons.filled.LocationOn
import androidx.compose.material.icons.filled.MoreVert
import androidx.compose.material3.DropdownMenu
import androidx.compose.material3.DropdownMenuItem
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.material3.TopAppBar
import androidx.compose.material3.TopAppBarDefaults
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        setContent {
            MaterialTheme {
                OpenMailScreen()
            }
        }
    }

    @Preview(showSystemUi = true)
    @OptIn(ExperimentalMaterial3Api::class)
    @Composable
    fun OpenMailScreen() {
        // Добавил текст который меняется при нажатии на пользователя
        var mainText by remember {
            mutableStateOf("It’s HERE! NEW Deals new you!\nCome check out the\nexcitement!")
        }

        // Добавил состояние для открытия меню с тремя точками
        var menuExpanded by remember { mutableStateOf(false) }

        // Добавил состояние темы
        var isDarkTheme by remember { mutableStateOf(false) }

        val backgroundColor = if (isDarkTheme) Color.Black else Color.White
        val textColor = if (isDarkTheme) Color.White else Color.Black

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(backgroundColor)
        ) {
            TopAppBar(
                title = {
                    Text(
                        text = "AppSecretShop",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color.Black
                    )
                },
                actions = {
                    IconButton(
                        onClick = {
                            // Добавил сообщение для кнопки местоположения
                            Toast.makeText(
                                this@MainActivity,
                                "Пока не доступно",
                                Toast.LENGTH_SHORT
                            ).show()
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.LocationOn,
                            contentDescription = "Местоположение",
                            tint = Color.Black
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил сообщение для кнопки письма
                            Toast.makeText(
                                this@MainActivity,
                                "Пока писем нет",
                                Toast.LENGTH_SHORT
                            ).show()
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.Email,
                            contentDescription = "Письмо",
                            tint = Color.Black
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил замену текста при нажатии на пользователя
                            mainText = "Сейчас пользователь это Вы!"
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.AccountCircle,
                            contentDescription = "Пользователь",
                            tint = Color.Black
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил открытие контекстного меню
                            menuExpanded = true
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.MoreVert,
                            contentDescription = "Меню",
                            tint = Color.Black
                        )
                    }

                    DropdownMenu(
                        expanded = menuExpanded,
                        onDismissRequest = {
                            menuExpanded = false
                        }
                    ) {
                        DropdownMenuItem(
                            text = {
                                Text(text = "Светлая тема")
                            },
                            onClick = {
                                // Добавил светлую тему
                                isDarkTheme = false
                                menuExpanded = false
                            }
                        )

                        DropdownMenuItem(
                            text = {
                                Text(text = "Темная тема")
                            },
                            onClick = {
                                // Добавил темную тему
                                isDarkTheme = true
                                menuExpanded = false
                            }
                        )
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = Color(0xFF4FC3D7)
                ),
                modifier = Modifier.fillMaxWidth()
            )

            Text(
                text = mainText,
                fontSize = 26.sp,
                fontWeight = FontWeight.Bold,
                color = textColor,
                modifier = Modifier.padding(start = 16.dp, top = 16.dp, end = 16.dp)
            )
        }
    }
}
```

Creating a Registration Screen in Kotlin using XML and ViewModel While Jetpack Compose is the future of Android, the traditional XML View system combined with View Binding and a ViewModel remains a robust, industry-standard way to build a registration window. This approach ensures a clean separation of concerns between your UI and your business logic.

  The Architecture (MVVM) To make the registration window scalable and testable, we divide the responsibility:
XML Layout: Defines the visual structure (EditTexts, Buttons).

  Activity/Fragment: Handles UI interactions and observes data.
  
 ViewModel: Validates inputs (e.g., checking if the email is valid or if passwords match) and communicates with the backend.

  Step-by-Step Implementation The XML Layout (activity_register.xml) We use TextInputLayout from the Material Components library to get those smooth floating labels and built-in error placeholders.
```
Menu options 2


package com.example.dialogv1

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.padding
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Delete
import androidx.compose.material.icons.filled.Email
import androidx.compose.material.icons.filled.MoreVert
import androidx.compose.material3.DropdownMenu
import androidx.compose.material3.DropdownMenuItem
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.material3.TopAppBar
import androidx.compose.material3.TopAppBarDefaults
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        setContent {
            MaterialTheme {
                OpenMailScreen()
            }
        }
    }

    @Preview(showSystemUi = true)
    @OptIn(ExperimentalMaterial3Api::class)
    @Composable
    fun OpenMailScreen() {
        // Добавил текст который меняется при нажатии на корзину
        var mainText by remember {
            mutableStateOf("It’s HERE! NEW Deals new you!\nCome check out the\nexcitement!")
        }

        // Добавил состояние открытия меню
        var menuExpanded by remember { mutableStateOf(false) }

        // Добавил состояние темы
        var isDarkTheme by remember { mutableStateOf(false) }

        val backgroundColor = if (isDarkTheme) Color.Black else Color.White
        val textColor = if (isDarkTheme) Color.White else Color.Black
        val toolbarColor = if (isDarkTheme) Color.DarkGray else Color(0xFFD6D6D6)
        val toolbarTextColor = if (isDarkTheme) Color.White else Color.Black
        val iconColor = if (isDarkTheme) Color.White else Color.Black

        Column(
            modifier = Modifier
                .fillMaxSize()
                .background(backgroundColor)
        ) {
            TopAppBar(
                title = {
                    Text(
                        text = "AppSecretShop",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold,
                        color = toolbarTextColor
                    )
                },
                actions = {
                    IconButton(
                        onClick = {
                            // Добавил сообщение для кнопки архив
                            Toast.makeText(
                                this@MainActivity,
                                "Добавлено в архив",
                                Toast.LENGTH_SHORT
                            ).show()
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.Email,
                            contentDescription = "Архив",
                            tint = iconColor
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил замену текста при нажатии на корзину
                            mainText = "Удалено"
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.Delete,
                            contentDescription = "Корзина",
                            tint = iconColor
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил сообщение для кнопки письмо
                            Toast.makeText(
                                this@MainActivity,
                                "Пока писем нет",
                                Toast.LENGTH_SHORT
                            ).show()
                        }
                    ) {
                        Icon(
                            imageVector = Icons.Default.Email,
                            contentDescription = "Письмо",
                            tint = iconColor
                        )
                    }

                    Box {
                        IconButton(
                            onClick = {
                                // Добавил открытие контекстного меню
                                menuExpanded = true
                            }
                        ) {
                            Icon(
                                imageVector = Icons.Default.MoreVert,
                                contentDescription = "Меню",
                                tint = iconColor
                            )
                        }

                        DropdownMenu(
                            expanded = menuExpanded,
                            onDismissRequest = {
                                menuExpanded = false
                            }
                        ) {
                            DropdownMenuItem(
                                text = {
                                    Text(text = "Светлая тема")
                                },
                                onClick = {
                                    // Добавил светлую тему
                                    isDarkTheme = false
                                    menuExpanded = false
                                }
                            )

                            DropdownMenuItem(
                                text = {
                                    Text(text = "Темная тема")
                                },
                                onClick = {
                                    // Добавил темную тему
                                    isDarkTheme = true
                                    menuExpanded = false
                                }
                            )
                        }
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = toolbarColor
                ),
                modifier = Modifier.fillMaxWidth()
            )

            Box(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 16.dp, top = 16.dp, end = 16.dp)
            ) {
                Text(
                    text = mainText,
                    fontSize = 26.sp,
                    fontWeight = FontWeight.Bold,
                    color = textColor,
                    modifier = Modifier.align(Alignment.TopStart)
                )

                Text(
                    text = "☆",
                    fontSize = 30.sp,
                    color = textColor,
                    modifier = Modifier.align(Alignment.TopEnd)
                )
            }
        }
    }
}
```


Creating a Web Registration Window in Kotlin using Ktor and HTML DSL Kotlin isn't just for mobile and desktop apps; it's a powerful tool for server-side web development. Using Ktor (JetBrains' asynchronous web framework) and the Kotlinx.html DSL, you can generate HTML forms dynamically on the server side with total type safety.

The Full-Stack Kotlin Concept Instead of writing HTML files manually, Kotlin allows you to write HTML structure directly inside your backend routing using pure Kotlin code. This makes sharing data classes (like user registration models) between your frontend template and your database completely seamless.

  MENU BOTTOM EXAMPLE 

  ```
Menu bottom 1


package com.example.dialogv1

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.Icon
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.NavigationBar
import androidx.compose.material3.NavigationBarItem
import androidx.compose.material3.NavigationBarItemDefaults
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Home
import androidx.compose.material.icons.filled.Person
import androidx.compose.material.icons.filled.Notifications
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        setContent {
            MaterialTheme {
                BottomMenuScreen()
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun BottomMenuScreen() {
        // Добавил выбранный пункт нижнего меню
        var selectedItem by remember { mutableStateOf("Home") }

        // Добавил текст который меняется при нажатии на пункты меню
        var welcomeText by remember { mutableStateOf("Welcome to the Home") }

        Scaffold(
            bottomBar = {
                NavigationBar(
                    containerColor = Color.White
                ) {
                    NavigationBarItem(
                        selected = selectedItem == "Home",
                        onClick = {
                            // Добавил обработку кнопки Home
                            selectedItem = "Home"
                            welcomeText = "Welcome to the Home"
                        },
                        icon = {
                            Icon(
                                imageVector = Icons.Default.Home,
                                contentDescription = "Home"
                            )
                        },
                        label = {
                            Text(text = "Home")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF0B6EFE),
                            selectedTextColor = Color(0xFF0B6EFE),
                            unselectedIconColor = Color(0xFF9E9E9E),
                            unselectedTextColor = Color(0xFF9E9E9E),
                            indicatorColor = Color.Transparent
                        )
                    )

                    NavigationBarItem(
                        selected = selectedItem == "Wallet",
                        onClick = {
                            // Добавил обработку кнопки Wallet
                            selectedItem = "Wallet"
                            welcomeText = "Welcome to the Wallet"
                        },
                        icon = {
                            Text(
                                text = "▣",
                                fontSize = 22.sp
                            )
                        },
                        label = {
                            Text(text = "Wallet")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF0B6EFE),
                            selectedTextColor = Color(0xFF0B6EFE),
                            unselectedIconColor = Color(0xFF9E9E9E),
                            unselectedTextColor = Color(0xFF9E9E9E),
                            indicatorColor = Color.Transparent
                        )
                    )

                    NavigationBarItem(
                        selected = selectedItem == "Track",
                        onClick = {
                            // Добавил обработку кнопки Track
                            selectedItem = "Track"
                            welcomeText = "Welcome to the Track"
                        },
                        icon = {
                            Text(
                                text = "♙",
                                fontSize = 22.sp
                            )
                        },
                        label = {
                            Text(text = "Track")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF0B6EFE),
                            selectedTextColor = Color(0xFF0B6EFE),
                            unselectedIconColor = Color(0xFF9E9E9E),
                            unselectedTextColor = Color(0xFF9E9E9E),
                            indicatorColor = Color.Transparent
                        )
                    )

                    NavigationBarItem(
                        selected = selectedItem == "Profile",
                        onClick = {
                            // Добавил обработку кнопки Profile
                            selectedItem = "Profile"
                            welcomeText = "Welcome to the Profile"
                        },
                        icon = {
                            Icon(
                                imageVector = Icons.Default.Person,
                                contentDescription = "Profile"
                            )
                        },
                        label = {
                            Text(text = "Profile")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF0B6EFE),
                            selectedTextColor = Color(0xFF0B6EFE),
                            unselectedIconColor = Color(0xFF9E9E9E),
                            unselectedTextColor = Color(0xFF9E9E9E),
                            indicatorColor = Color.Transparent
                        )
                    )
                }
            }
        ) { paddingValues ->
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .background(Color.White)
                    .padding(paddingValues),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(110.dp))

                Box(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(horizontal = 24.dp)
                        .height(84.dp)
                        .clip(RoundedCornerShape(8.dp))
                        .background(Color(0xFF0B6EFE))
                ) {
                    Box(
                        modifier = Modifier
                            .align(Alignment.CenterStart)
                            .padding(start = 14.dp)
                            .size(56.dp)
                            .clip(CircleShape)
                            .background(Color(0xFF1A1A1A))
                    )

                    Column(
                        modifier = Modifier
                            .align(Alignment.CenterStart)
                            .padding(start = 84.dp)
                    ) {
                        Text(
                            text = "Hello Ken!",
                            fontSize = 24.sp,
                            fontWeight = FontWeight.Bold,
                            color = Color.White
                        )

                        Text(
                            text = welcomeText,
                            fontSize = 14.sp,
                            color = Color.White
                        )
                    }

                    Icon(
                        imageVector = Icons.Default.Notifications,
                        contentDescription = "Notification",
                        tint = Color.White,
                        modifier = Modifier
                            .align(Alignment.CenterEnd)
                            .padding(end = 14.dp)
                    )
                }
            }
        }
    }
}
  ```

  Highlights of Web-Based Kotlin UI Type-Safe HTML (DSL): Notice how div, form, and textInput are regular Kotlin blocks. If you misspell an HTML tag, your project won't compile, saving you from standard web typos. Tailwind CSS Integration: By including the Tailwind library in the , we can style elements explicitly using clean utility strings like bg-gray-100 (light background) and shadow-md (drop shadow effect) right within Kotlin.   Native Form Handling: The Ktor backend processes inbound data asynchronously using call.receiveParameters(), seamlessly binding HTTP parameters to backend variables.

  EXAMPLE 2 FOR US
```
  Menu bottom 2

package com.example.dialogv1

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.border
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.DateRange
import androidx.compose.material.icons.filled.Home
import androidx.compose.material.icons.filled.Person
import androidx.compose.material3.Icon
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.NavigationBar
import androidx.compose.material3.NavigationBarItem
import androidx.compose.material3.NavigationBarItemDefaults
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        setContent {
            MaterialTheme {
                BottomMenuScreen()
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun BottomMenuScreen() {
        // Добавил выбранный пункт нижнего меню
        var selectedItem by remember { mutableStateOf("Home") }

        // Добавил текст который меняется при нажатии на кнопки меню
        var descriptionText by remember {
            mutableStateOf("Thank you for visiting the page Home")
        }

        Scaffold(
            bottomBar = {
                NavigationBar(
                    containerColor = Color.White
                ) {
                    NavigationBarItem(
                        selected = selectedItem == "Home",
                        onClick = {
                            // Добавил обработку кнопки Home
                            selectedItem = "Home"
                            descriptionText = "Thank you for visiting the page Home"
                        },
                        icon = {
                            Icon(
                                imageVector = Icons.Default.Home,
                                contentDescription = "Home"
                            )
                        },
                        label = {
                            Text(text = "Home")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF006FFD),
                            selectedTextColor = Color(0xFF006FFD),
                            unselectedIconColor = Color.Black,
                            unselectedTextColor = Color.Black,
                            indicatorColor = Color.Transparent
                        )
                    )

                    NavigationBarItem(
                        selected = selectedItem == "Chat",
                        onClick = {
                            // Добавил обработку кнопки Chat
                            selectedItem = "Chat"
                            descriptionText = "Thank you for visiting the page Chat"
                        },
                        icon = {
                            Text(
                                text = "☏",
                                fontSize = 26.sp
                            )
                        },
                        label = {
                            Text(text = "Chat")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF006FFD),
                            selectedTextColor = Color(0xFF006FFD),
                            unselectedIconColor = Color.Black,
                            unselectedTextColor = Color.Black,
                            indicatorColor = Color.Transparent
                        )
                    )

                    NavigationBarItem(
                        selected = selectedItem == "Profile",
                        onClick = {
                            // Добавил обработку кнопки Profile
                            selectedItem = "Profile"
                            descriptionText = "Thank you for visiting the page Profile"
                        },
                        icon = {
                            Icon(
                                imageVector = Icons.Default.Person,
                                contentDescription = "Profile"
                            )
                        },
                        label = {
                            Text(text = "Profile")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF006FFD),
                            selectedTextColor = Color(0xFF006FFD),
                            unselectedIconColor = Color.Black,
                            unselectedTextColor = Color.Black,
                            indicatorColor = Color.Transparent
                        )
                    )

                    NavigationBarItem(
                        selected = selectedItem == "Calendar",
                        onClick = {
                            // Добавил обработку кнопки Calendar
                            selectedItem = "Calendar"
                            descriptionText = "Thank you for visiting the page Calendar"
                        },
                        icon = {
                            Icon(
                                imageVector = Icons.Default.DateRange,
                                contentDescription = "Calendar"
                            )
                        },
                        label = {
                            Text(text = "Calendar")
                        },
                        colors = NavigationBarItemDefaults.colors(
                            selectedIconColor = Color(0xFF006FFD),
                            selectedTextColor = Color(0xFF006FFD),
                            unselectedIconColor = Color.Black,
                            unselectedTextColor = Color.Black,
                            indicatorColor = Color.Transparent
                        )
                    )
                }
            }
        ) { paddingValues ->
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .background(Color.White)
                    .padding(paddingValues),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Spacer(modifier = Modifier.height(170.dp))

                Row(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(horizontal = 32.dp)
                        .height(78.dp)
                        .clip(RoundedCornerShape(8.dp))
                        .border(
                            width = 1.dp,
                            color = Color(0xFFBDBDBD),
                            shape = RoundedCornerShape(8.dp)
                        )
                        .background(Color.White)
                        .padding(horizontal = 12.dp),
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    // Добавил круглый аватар
                    Box(
                        modifier = Modifier
                            .size(54.dp)
                            .clip(CircleShape)
                            .background(Color(0xFFE6F0FF)),
                        contentAlignment = Alignment.Center
                    ) {
                        Text(
                            text = "👨",
                            fontSize = 30.sp
                        )
                    }

                    Column(
                        modifier = Modifier
                            .weight(1f)
                            .padding(start = 12.dp)
                    ) {
                        Text(
                            text = "Raph Ron",
                            fontSize = 16.sp,
                            fontWeight = FontWeight.Bold,
                            color = Color.Black
                        )

                        Text(
                            text = descriptionText,
                            fontSize = 11.sp,
                            color = Color(0xFF333333)
                        )
                    }

                    // Добавил синий круг с цифрой
                    Box(
                        modifier = Modifier
                            .size(28.dp)
                            .clip(CircleShape)
                            .background(Color(0xFF006FFD)),
                        contentAlignment = Alignment.Center
                    ) {
                        Text(
                            text = "5",
                            fontSize = 14.sp,
                            color = Color.White
                        )
                    }
                }
            }
        }
    }
}
```
