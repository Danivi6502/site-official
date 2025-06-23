<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>POST ESCOLA</title>
    <style>
        body { font-family: Arial, sans-serif; max-width: 900px; margin: auto; padding: 20px; display: flex; }
        .sidebar {
            position: fixed;
            left: 0px;
            top: 20px;
            width: 220px;
            padding: 10px;
            max-height: calc(100vh - 0px);
            overflow-y: auto;
            box-sizing: border-box;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .sidebar-content {
            flex-grow: 1;
        }
        .language-selector-container {
            padding-top: 20px;
            border-top: 1px solid #eee;
            margin-top: auto;
        }
        #languageSelect {
            width: 100%;
            padding: 8px;
            margin-top: 10px;
            border-radius: 4px;
            border: 1px solid #ccc;
            background-color: white;
            font-size: 14px;
        }
        .dark-mode #languageSelect {
            background-color: #555;
            color: white;
            border-color: #777;
        }

        .content { margin-left: 240px; width: calc(100% - 260px); padding-left: 20px; }
        #searchBar {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        .dark-mode #searchBar {
            background-color: #444;
            color: white;
            border: 1px solid #666;
        }

        input, textarea, button { width: 100%; margin-bottom: 10px; padding: 10px; border: none; }
        button { cursor: pointer; background-color: #1DA1F2; color: white; }
        .post { background: white; padding: 10px; margin-top: 10px; border-radius: 5px; }
        .post-actions {
            display: flex;
            gap: 5px;
            margin-top: 5px;
        }
        .delete-btn, .reply-btn, .edit-btn-post { background-color: red; color: white; border: none; padding: 5px; cursor: pointer; }
        .reply-btn, .edit-btn-post { background-color: #4CAF50; }
        .reply-btn { margin-left: 0; }


        .reply-box { display: none; margin-top: 5px; }
        .reply-box input[type="file"] {
            width: auto;
            margin-bottom: 5px;
            padding: 5px;
            font-size: 13px;
        }

        .status { font-weight: bold; }
        .replies { margin-left: 20px; border-left: 2px solid #ddd; padding-left: 10px; }
        .replies img {
            max-width: 100%;
            height: auto;
            margin-top: 5px;
            border-radius: 3px;
        }

        img { max-width: 100%; margin-top: 10px; }
        .dark-mode { background-color: #333; color: white; }
        .dark-mode .post, .dark-mode input, .dark-mode textarea, .dark-mode button { background-color: #444; color: white; border: 1px solid #666; }

        .dark-mode .sidebar::-webkit-scrollbar {
            width: 8px;
        }

        .dark-mode .sidebar::-webkit-scrollbar-track {
            background: #555;
            border-radius: 10px;
        }

        .dark-mode .sidebar::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 10px;
        }

        .dark-mode .sidebar::-webkit-scrollbar-thumb:hover {
            background: #aaa;
        }


        .user-controls {
            display: flex;
            gap: 5px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        .user-controls button {
            flex: 1;
            background-color: #4CAF50;
            padding: 8px;
            font-size: 14px;
        }
        .user-controls button.delete-account {
            background-color: #f44336;
        }
        .profile-picture-container {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
            gap: 10px;
        }
        .profile-picture-container img {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            object-fit: cover;
            border: 2px solid #1DA1F2;
        }
        .profile-picture-container span {
            font-weight: bold;
        }
        #profilePictureInput {
            width: auto;
            margin-bottom: 5px;
            padding: 5px;
            font-size: 13px;
        }
        #removeProfilePictureBtn {
            background-color: #f44336;
            width: auto;
            padding: 5px 10px;
            font-size: 13px;
            margin-bottom: 10px;
        }

        #userBio {
            font-size: 0.9em;
            color: #666;
            margin-top: 5px;
            margin-bottom: 10px;
            white-space: pre-wrap;
        }
        .dark-mode #userBio {
            color: #ccc;
        }
        #bioInput {
            margin-bottom: 5px;
            height: 60px;
            resize: vertical;
        }
        #editBioBtn {
            background-color: #007bff;
            width: auto;
            padding: 5px 10px;
            font-size: 13px;
            margin-bottom: 10px;
        }


        #adminPanel {
            margin-top: 20px;
            border: 1px solid #888;
            padding: 10px;
            max-height: 300px;
            overflow-y: auto;
            background: #f9f9f9;
            color: #000;
        }
        .dark-mode #adminPanel {
            background: #444;
            border-color: #666;
            color: #eee;
        }
        #adminPanel h3 {
            margin-top: 0;
        }
        .admin-user {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 6px 5px;
            border-bottom: 1px solid #ddd;
            font-size: 14px;
        }
        .dark-mode .admin-user {
            border-color: #555;
        }
        .admin-user button {
            margin-left: 5px;
            padding: 4px 8px;
            font-size: 13px;
            cursor: pointer;
        }
        .admin-user button.delete-btn {
            background-color: #f44336;
            color: white;
            border: none;
        }
        .admin-user button.edit-btn {
            background-color: #4CAF50;
            color: white;
            border: none;
        }

        #editModal, #editReplyModal, #editChatMessageModal, #editStatusModal {
            display: none;
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.6);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        #editModalContent, #editReplyModalContent, #editChatMessageModalContent, #editStatusModalContent {
            background: white;
            padding: 20px;
            border-radius: 8px;
            max-width: 400px;
            width: 90%;
            box-sizing: border-box;
        }
        #editModalContent.dark-mode, #editReplyModalContent.dark-mode, #editChatMessageModalContent.dark-mode, #editStatusModalContent.dark-mode {
            background: #444;
            color: white;
        }
        #editModalContent input[type="file"],
        #editModalContent textarea,
        #editReplyModalContent textarea,
        #editChatMessageModalContent textarea,
        #editChatMessageModalContent input[type="file"],
        #editStatusModalContent textarea,
        #editStatusModalContent input[type="file"] {
            width: 100%;
            margin-bottom: 10px;
            padding: 8px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 14px;
        }
        #editModalContent button,
        #editReplyModalContent button,
        #editChatMessageModalContent button,
        #editStatusModalContent button {
            width: 48%;
            padding: 10px;
            font-size: 14px;
            cursor: pointer;
        }
        #editModalContent button.cancel-btn,
        #editReplyModalContent button.cancel-btn,
        #editChatMessageModalContent button.cancel-btn,
        #editStatusModalContent button.cancel-btn {
            background: #f44336;
            color: white;
            border: none;
        }
        #editModalContent button.save-btn,
        #editReplyModalContent button.save-btn,
        #editChatMessageModalContent button.save-btn,
        #editStatusModalContent button.save-btn {
            background: #4CAF50;
            color: white;
            border: none;
            float: right;
        }

        .reply-box textarea {
            width: 100%;
            padding: 6px;
            font-size: 14px;
            margin-bottom: 6px;
        }
        .reply-box button {
            background-color: #1DA1F2;
            border: none;
            color: white;
            padding: 6px 10px;
            cursor: pointer;
            font-size: 14px;
        }
        .reply-text-and-actions {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            width: 100%;
        }
        .reply-content-wrapper {
            width: 100%;
        }
        .reply-text-and-actions span {
            flex-grow: 1;
        }
        .reply-actions {
            display: flex;
            gap: 5px;
            margin-top: 5px;
            margin-left: auto;
        }
        .reply-actions button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 3px 6px;
            font-size: 12px;
            cursor: pointer;
        }
        .reply-actions button.delete-btn {
            background-color: red;
        }

        #friendsSection {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
        #friendsSection h4 {
            margin-top: 10px;
            margin-bottom: 5px;
        }
        #friendsList, #sentRequestsList, #receivedRequestsList {
            list-style: none;
            padding: 0;
            margin-top: 10px;
        }
        #friendsList li, #sentRequestsList li, #receivedRequestsList li {
            padding: 5px 0;
            border-bottom: 1px dotted #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #friendsList li:last-child, #sentRequestsList li:last-child, #receivedRequestsList li:last-child {
            border-bottom: none;
        }
        #friendsList li button, #sentRequestsList li button, #receivedRequestsList li button {
            width: auto;
            padding: 3px 8px;
            margin-left: 5px;
            font-size: 12px;
        }
        #friendsList li button {
            background-color: #f44336;
        }
        #receivedRequestsList .accept-btn {
            background-color: #4CAF50;
            margin-right: 5px;
        }
        #receivedRequestsList .reject-btn {
            background-color: #f44336;
        }
        #addFriendInput, #sendFriendRequestButton {
            margin-top: 5px;
            margin-bottom: 5px;
        }

        #chatSection {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
        #chatFriendsList {
            list-style: none;
            padding: 0;
            margin-top: 10px;
            max-height: 150px;
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: 4px;
        }
        #chatFriendsList li {
            padding: 8px 10px;
            cursor: pointer;
            border-bottom: 1px dotted #eee;
        }
        #chatFriendsList li:hover {
            background-color: #f0f0f0;
        }
        .dark-mode #chatFriendsList li:hover {
            background-color: #555;
        }
        #chatFriendsList li.selected {
            background-color: #e0e0e0;
            font-weight: bold;
        }
        .dark-mode #chatFriendsList li.selected {
            background-color: #666;
        }

        #chatBox {
            border: 1px solid #ddd;
            padding: 10px;
            margin-top: 15px;
            border-radius: 5px;
            display: none;
        }
        #chatHeader {
            font-weight: bold;
            margin-bottom: 10px;
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #chatHeader span {
            flex-grow: 1;
        }
        #closeChatBtn {
            background-color: #f44336;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
            width: auto;
            margin-bottom: 0;
        }
        #messagesDisplay {
            max-height: 250px;
            overflow-y: auto;
            border: 1px solid #eee;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 4px;
        }
        .message {
            margin-bottom: 5px;
            display: flex;
            flex-direction: column;
            align-items: flex-start;
            position: relative;
        }
        .message strong {
            color: #1DA1F2;
        }
        .message.mine {
            text-align: right;
            align-items: flex-end;
        }
        .message.mine strong {
            color: #4CAF50;
        }
        .message img {
            max-width: 80%;
            height: auto;
            display: block;
            margin-top: 5px;
            border-radius: 3px;
        }
        .message.mine img {
            margin-left: auto;
            margin-right: 0;
        }
        .message-content {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
        }
        .message.mine .message-content {
            align-items: flex-end;
        }

        .message-options {
            position: absolute;
            top: 5px;
            right: 5px;
            display: none;
        }
        .message.mine .message-options {
            left: 5px;
            right: auto;
        }
        .message:hover .message-options {
            display: flex;
            gap: 3px;
        }
        .message-options button {
            background-color: #ccc;
            color: #333;
            border: none;
            padding: 2px 5px;
            font-size: 10px;
            cursor: pointer;
            width: auto;
            margin-bottom: 0;
            border-radius: 3px;
        }
        .dark-mode .message-options button {
            background-color: #666;
            color: #eee;
        }
        .message-options .delete-btn {
            background-color: #f44336;
            color: white;
        }
        .message-options .edit-btn {
            background-color: #4CAF50;
            color: white;
        }


        #chatInputContainer {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        #chatMessageInput {
            flex-grow: 1;
            margin-bottom: 0;
        }
        #sendChatMessageBtn {
            width: auto;
            padding: 10px 15px;
            margin-bottom: 0;
        }
        #chatImageInput {
            width: auto;
            flex-basis: 100%;
            box-sizing: border-box;
            font-size: 13px;
            padding: 5px;
            margin-bottom: 0;
        }


        #groupSection {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
        #createGroupForm {
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px dotted #eee;
        }
        #groupMembersList {
            list-style: none;
            padding: 0;
            max-height: 120px;
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: 4px;
            margin-bottom: 10px;
        }
        #groupMembersList li {
            padding: 5px 10px;
            display: flex;
            align-items: center;
        }
        #groupMembersList li input[type="checkbox"] {
            width: auto;
            margin-right: 8px;
        }
        #myGroupsList {
            list-style: none;
            padding: 0;
            max-height: 150px;
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: 4px;
        }
        #myGroupsList li {
            padding: 8px 10px;
            border-bottom: 1px dotted #eee;
            cursor: pointer;
        }
        #myGroupsList li:last-child {
            border-bottom: none;
        }
        #myGroupsList li.selected {
            background-color: #e0e0e0;
            font-weight: bold;
        }
        .dark-mode #myGroupsList li.selected {
            background-color: #666;
        }
        .dark-mode #myGroupsList li:hover {
            background-color: #555;
        }

        #groupChatBox {
            border: 1px solid #ddd;
            padding: 10px;
            margin-top: 15px;
            border-radius: 5px;
            display: none;
        }
        #groupChatHeader {
            font-weight: bold;
            margin-bottom: 10px;
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #groupChatHeader span {
            flex-grow: 1;
        }
        #closeGroupChatBtn {
            background-color: #f44336;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
            width: auto;
            margin-bottom: 0;
        }
        #groupMessagesDisplay {
            max-height: 250px;
            overflow-y: auto;
            border: 1px solid #eee;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 4px;
        }
        #groupChatInputContainer {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        #groupChatMessageInput {
            flex-grow: 1;
            margin-bottom: 0;
        }
        #sendGroupChatMessageBtn {
            width: auto;
            padding: 10px 15px;
            margin-bottom: 0;
        }
        #groupChatImageInput {
            width: auto;
            flex-basis: 100%;
            box-sizing: border-box;
            font-size: 13px;
            padding: 5px;
            margin-bottom: 0;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 5px;
        }
        .user-info img.profile-pic-small {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            object-fit: cover;
            border: 1px solid #ddd;
        }
        .user-info strong {
            margin-bottom: 0;
            cursor: pointer; /* Makes username clickable */
        }
        .user-info img.profile-pic-small {
            cursor: pointer; /* Makes profile pic clickable */
        }
        .message .user-info {
            margin-bottom: 0;
        }

        /* NEW: User Profile View Styles */
        #userProfileView {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
            display: none; /* Hidden by default */
        }
        #userProfileView h3 {
            margin-top: 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #userProfileView .close-btn {
            background: none;
            border: none;
            color: #f44336;
            font-size: 1.2em;
            cursor: pointer;
            width: auto;
            padding: 0;
            margin: 0;
        }
        #userProfileView .profile-details {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: 5px;
            text-align: center;
        }
        .dark-mode #userProfileView .profile-details {
            border-color: #555;
            background-color: #444;
        }
        #userProfileView .profile-details img {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            object-fit: cover;
            border: 3px solid #1DA1F2;
            margin-bottom: 10px;
        }
        #userProfileView .profile-details h4 {
            margin: 5px 0;
        }
        #userProfileView .profile-details p {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 10px;
            white-space: pre-wrap;
        }
        .dark-mode #userProfileView .profile-details p {
            color: #ccc;
        }
        #userProfileView .profile-details button {
            width: auto;
            padding: 8px 15px;
            font-size: 14px;
            margin-top: 10px;
        }
        #userProfileView .profile-details button.remove-friend-btn {
            background-color: #f44336;
        }
        #userProfileView .profile-details button.pending-request-btn {
            background-color: #ff9800;
            cursor: default;
        }
        #userProfileView .profile-details button.received-request-accept-btn {
            background-color: #4CAF50;
            margin-right: 5px;
        }
        #userProfileView .profile-details button.received-request-reject-btn {
            background-color: #f44336;
        }
        #userProfileView .profile-details button.chat-btn {
            background-color: #1DA1F2;
        }
        #userProfileView .profile-details .button-group {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        /* NEW: Status Section */
        #statusSection {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
        #statusInputContainer {
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px dotted #eee;
        }
        #statusTextInput {
            height: 50px;
            resize: vertical;
            margin-bottom: 5px;
        }
        #statusImageInput {
            width: auto;
            padding: 5px;
            font-size: 13px;
            margin-bottom: 5px;
        }
        #postStatusBtn {
            margin-bottom: 0;
        }

        #myStatusDisplay {
            margin-top: 10px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            display: none; /* Hidden if no status */
            position: relative;
        }
        #myStatusDisplay .status-content {
            margin-bottom: 5px;
        }
        #myStatusDisplay img {
            max-width: 100%;
            height: auto;
            border-radius: 3px;
            margin-top: 5px;
        }
        #myStatusDisplay .status-time {
            font-size: 0.8em;
            color: #888;
            margin-top: 5px;
        }
        .dark-mode #myStatusDisplay .status-time {
            color: #bbb;
        }
        #myStatusDisplay .status-actions {
            position: absolute;
            top: 5px;
            right: 5px;
            display: flex;
            gap: 5px;
        }
        #myStatusDisplay .status-actions button {
            width: auto;
            padding: 3px 8px;
            font-size: 12px;
            margin: 0;
        }
        #myStatusDisplay .status-actions .delete-btn {
            background-color: #f44336;
        }
        #myStatusDisplay .status-actions .edit-btn {
            background-color: #4CAF50;
        }

        #friendStatusesSection {
            margin-top: 20px;
            border-top: 1px solid #ccc;
            padding-top: 10px;
        }
        #friendStatusesList {
            list-style: none;
            padding: 0;
        }
        #friendStatusesList li {
            padding: 10px;
            border: 1px solid #eee;
            border-radius: 5px;
            margin-bottom: 10px;
            background-color: #f9f9f9;
        }
        .dark-mode #friendStatusesList li {
            background-color: #444;
            border-color: #555;
        }
        #friendStatusesList li .user-info {
            margin-bottom: 10px;
        }
        #friendStatusesList li .status-content {
            margin-bottom: 5px;
        }
        #friendStatusesList li img {
            max-width: 100%;
            height: auto;
            border-radius: 3px;
            margin-top: 5px;
        }
        #friendStatusesList li .status-time {
            font-size: 0.8em;
            color: #888;
            margin-top: 5px;
        }
        .dark-mode #friendStatusesList li .status-time {
            color: #bbb;
        }
    </style>
</head>
<body>

    <div class="sidebar">
        <div class="sidebar-content">
            <button class="mode-btn" onclick="toggleMode()"></button>
            <h2 id="loginTitle"></h2>
            <div class="profile-picture-container">
                <img id="sidebarProfilePicture" src="default-profile.png" alt="Foto de Perfil" />
                <span id="statusText"></span>
            </div>
            <p id="userBio"></p>

            <input type="file" id="profilePictureInput" accept="image/*" style="display:none;" />
			
            <button id="uploadProfilePictureBtn" onclick="triggerProfilePictureInput()" style="display:none;"></button>
			
            <button id="removeProfilePictureBtn" onclick="removeProfilePicture()" style="display:none;"></button>
			<textarea id="bioInput" placeholder="Sua biografia (opcional)"></textarea>
			</br>

            <input type="text" id="usernameInput" />
            <input type="password" id="passwordInput" />
            
            <div class="user-controls">
                <button id="editUserBtn" onclick="editUser()"></button>
                <button id="editBioBtn" onclick="saveBio()"></button>
                <button id="deleteAccountBtn" class="delete-account" onclick="deleteAccount()"></button>
            </div>
            <button id="registerBtn" onclick="register()"></button>
            <button id="loginBtn" onclick="login()"></button>
            <button id="logoutBtn" onclick="logout()"></button>

            <div id="adminPanel" style="display:none;">
                <h3 id="adminPanelTitle"></h3>
                <div id="userList"></div>
            </div>

            <div id="friendsSection">
                <h3 id="friendsSectionTitle"></h3>
                <input type="text" id="addFriendInput" />
                <button id="sendFriendRequestButton" onclick="sendFriendRequest()"></button>

                <h4 id="myFriendsTitle"></h4>
                <ul id="friendsList"></ul>

                <h4 id="sentRequestsTitle"></h4>
                <ul id="sentRequestsList"></ul>

                <h4 id="receivedRequestsTitle"></h4>
                <ul id="receivedRequestsList"></ul>
            </div>

            <div id="groupSection" style="display:none;">
                <h3 id="groupSectionTitle"></h3>
                <div id="createGroupForm">
                    <h4 id="createGroupTitle"></h4>
                    <input type="text" id="groupNameInput" />
                    <p id="selectMembersText"></p>
                    <ul id="groupMembersList"></ul>
                    <button id="createGroupBtn" onclick="createGroup()"></button>
                </div>
                <h4 id="myGroupsTitle"></h4>
                <ul id="myGroupsList"></ul>
            </div>


            <div id="chatSection" style="display:none;">
                <h3 id="chatSectionTitle"></h3>
                <ul id="chatFriendsList"></ul>
            </div>

            <div id="userProfileView">
                <h3><span id="profileViewUsername"></span> <button class="close-btn" onclick="closeUserProfileView()">X</button></h3>
                <div class="profile-details">
                    <img id="profileViewPic" src="default-profile.png" alt="Profile Picture" />
                    <h4 id="profileViewName"></h4>
                    <p id="profileViewBio"></p>
                    <div id="profileViewActions" class="button-group">
                        </div>
                </div>
            </div>

            <div id="statusSection" style="display:none;">
                <h3 id="statusSectionTitle"></h3>
                <div id="statusInputContainer">
                    <h4 id="createStatusTitle"></h4>
                    <textarea id="statusTextInput" placeholder="O que você está pensando?"></textarea>
                    <input type="file" id="statusImageInput" accept="image/*" />
                    <button id="postStatusBtn" onclick="postStatus()"></button>
                </div>

                <h4 id="myStatusTitle"></h4>
                <div id="myStatusDisplay">
                    </div>

                <h4 id="friendStatusesTitle"></h4>
                <ul id="friendStatusesList">
                    </ul>
            </div>

        </div>
        <div class="language-selector-container">
            <label for="languageSelect" id="languageLabel"></label>
            <select id="languageSelect" onchange="setLanguage(this.value)">
                <option value="pt">Português</option>
                <option value="es">Español</option>
                <option value="en">English</option>
            </select>
        </div>
		</br>
    </div>

    <div class="content">
        <h2 id="postsTitle"></h2>
        <input type="text" id="searchBar" onkeyup="loadTweets()" placeholder="Pesquisar posts..." />

        <textarea id="tweetInput"></textarea>
        <input type="file" id="imageInput" accept="image/*" />
        <button id="postTweetBtn" onclick="postTweet()"></button>
        <div id="tweets"></div>

        <div id="chatBox">
            <div id="chatHeader">
                <span id="chatFriendName"></span>
                <button id="closeChatBtn" onclick="closeChat()">X</button>
            </div>
            <div id="messagesDisplay"></div>
            <div id="chatInputContainer">
                <input type="text" id="chatMessageInput" />
                <button id="sendChatMessageBtn" onclick="sendChatMessage()"></button>
                <input type="file" id="chatImageInput" accept="image/*" />
            </div>
        </div>

        <div id="groupChatBox">
            <div id="groupChatHeader">
                <span id="groupChatName"></span>
                <button id="closeGroupChatBtn" onclick="closeGroupChat()">X</button>
            </div>
            <div id="groupMessagesDisplay"></div>
            <div id="groupChatInputContainer">
                <input type="text" id="groupChatMessageInput" />
                <button id="sendGroupChatMessageBtn" onclick="sendGroupChatMessage()"></button>
                <input type="file" id="groupChatImageInput" accept="image/*" />
            </div>
        </div>
    </div>

    <div id="editModal">
        <div id="editModalContent">
            <h3 id="editPostTitle"></h3>
            <textarea id="editContent" rows="4"></textarea>
            <input type="file" id="editImageInput" accept="image/*" />
            <div style="display:flex; justify-content: space-between;">
                <button id="cancelEditPostBtn" class="cancel-btn" onclick="closeEditModal()"></button>
                <button id="saveEditPostBtn" class="save-btn" onclick="saveEdit()"></button>
            </div>
        </div>
    </div>

    <div id="editReplyModal">
        <div id="editReplyModalContent">
            <h3 id="editReplyTitle"></h3>
            <textarea id="editReplyContent" rows="3"></textarea>
            <input type="file" id="editReplyImageInput" accept="image/*" />
            <div style="display:flex; justify-content: space-between;">
                <button id="cancelEditReplyBtn" class="cancel-btn" onclick="closeEditReplyModal()"></button>
                <button id="saveEditReplyBtn" class="save-btn" onclick="saveReplyEdit()"></button>
            </div>
        </div>
    </div>

    <div id="editChatMessageModal">
        <div id="editChatMessageModalContent">
            <h3 id="editChatMessageTitle"></h3>
            <textarea id="editChatMessageContent" rows="3"></textarea>
            <input type="file" id="editChatMessageImageInput" accept="image/*" />
            <div style="display:flex; justify-content: space-between;">
                <button id="cancelEditChatMessageBtn" class="cancel-btn" onclick="closeEditChatMessageModal()"></button>
                <button id="saveEditChatMessageBtn" class="save-btn" onclick="saveChatMessageEdit()"></button>
            </div>
        </div>
    </div>

    <div id="editStatusModal">
        <div id="editStatusModalContent">
            <h3 id="editStatusTitle"></h3>
            <textarea id="editStatusContent" rows="3"></textarea>
            <input type="file" id="editStatusImageInput" accept="image/*" />
            <div style="display:flex; justify-content: space-between;">
                <button id="cancelEditStatusBtn" class="cancel-btn" onclick="closeEditStatusModal()"></button>
                <button id="saveEditStatusBtn" class="save-btn" onclick="saveStatusEdit()"></button>
            </div>
        </div>
    </div>

    <script>
        let currentUser = JSON.parse(localStorage.getItem("currentUser")) || null;
        let editPostId = null;
        let editPostImageData = null;
        let editReplyData = { postId: null, replyId: null, replyImageData: null };
        let currentChatFriend = null;
        let currentGroupChatId = null;

        let editChatData = {
            chatType: null,
            conversationId: null,
            messageId: null,
            messageImageData: null
        };
        // NEW: Status editing data
        let editStatusData = {
            statusId: null,
            statusImageData: null
        };


        const translations = {
            pt: {
                darkModeBtn: "Alternar Modo",
                loginTitle: "Login",
                statusNotLogged: "Status: Não logado",
                statusLoggedAs: "Logado como",
                uploadProfilePictureBtn: "Carregar/Alterar Foto",
                removeProfilePictureBtn: "Remover Foto",
                usernamePlaceholder: "Nome de usuário",
                passwordPlaceholder: "Senha",
                bioPlaceholder: "Sua biografia (opcional)",
                editUserBtn: "Editar nome e senha",
                editBioBtn: "Salvar Bio",
                deleteAccountBtn: "Excluir conta",
                registerBtn: "Registrar",
                loginBtn: "Entrar",
                logoutBtn: "Sair",
                adminPanelTitle: "Gerenciar Usuários (Admin)",
                adminDeleteBtn: "Excluir",
                adminEditBtn: "Editar Senha",
                friendsSectionTitle: "Gerenciar Amigos",
                addFriendInput: "Nome de usuário para solicitar",
                sendFriendRequestButton: "Enviar Solicitação",
                myFriendsTitle: "Meus Amigos",
                removeFriendBtn: "Remover",
                sentRequestsTitle: "Solicitações Enviadas",
                receivedRequestsTitle: "Solicitações Recebidas",
                acceptBtn: "Aceitar",
                rejectBtn: "Rejeitar",
                chatSectionTitle: "Conversar com Amigos",
                noFriendsChat: "Nenhum amigo para conversar.",
                chatHeader: "Conversa com",
                messageInputPlaceholder: "Digite sua mensagem...",
                sendBtn: "Enviar",
                noMessagesYet: "Nenhuma mensagem ainda. Comece a conversar!",
                groupSectionTitle: "Gerenciar Grupos",
                createGroupTitle: "Criar Novo Grupo",
                groupNameInput: "Nome do Grupo",
                selectMembersText: "Selecionar Membros:",
                createGroupBtn: "Criar Grupo",
                myGroupsTitle: "Meus Grupos",
                noGroupsYet: "Você não faz parte de nenhum grupo ainda.",
                groupChatHeader: "Chat do Grupo:",
                groupMessageInputPlaceholder: "Digite sua mensagem no grupo...",
                postsTitle: "Postagens",
                tweetInputPlaceholder: "O que está acontecendo?",
                postTweetBtn: "POSTAR",
                searchPlaceholder: "Pesquisar posts...",
                editPostTitle: "Editar Postagem",
                cancelBtn: "Cancelar",
                saveBtn: "Salvar",
                editReplyTitle: "Editar Resposta",
                editChatMessageTitle: "Editar Mensagem",
                you: "Você",
                editBtn: "Editar",
                deleteBtn: "Excluir",
                replyBtn: "Responder",
                replyInputPlaceholder: "Escreva sua resposta...",
                languageLabel: "Idioma:",
                alertLoginRequired: "Você precisa estar logado para ",
                alertFillFields: "Preencha nome de usuário e senha!",
                alertUserExists: "Usuário já existe!",
                alertRegistrationComplete: "Registro concluído! Agora faça login.",
                alertWelcome: "Bem vindo, ",
                alertInvalidCredentials: "Usuário ou senha incorretos.",
                alertPostSomething: "Digite algo ou escolha uma imagem para postar!",
                alertPostNotFound: "Postagem não encontrada.",
                alertDeletePostConfirm: "Excluir essa postagem?",
                alertReplySomething: "Digite algo ou escolha uma imagem para responder!",
                alertReplyNotFound: "Resposta não encontrada.",
                alertDeleteReplyConfirm: "Excluir esta resposta?",
                alertEmptyContent: "Conteúdo vazio não permitido.",
                alertSendRequestSelf: "Você não pode enviar uma solicitação de amizade para si mesmo!",
                alertUserNotExists: "O usuário não existe.",
                alertAlreadyFriend: " já é seu amigo.",
                alertRequestSentAlready: "Você já enviou uma solicitação de amizade para ",
                alertRequestReceivedAlready: "Você já recebeu uma solicitação de amizade de ",
                alertFriendRequestSent: "Solicitação de amizade enviada para ",
                alertAcceptFriendRequest: "Você aceitou a solicitação de amizade de ",
                alertNowFriends: ". Agora vocês são amigos!",
                alertRejectFriendRequestConfirm: "Tem certeza que deseja rejeitar a solicitação de ",
                alertRejectedFriendRequest: "Você rejeitou a solicitação de amizade de ",
                alertRemoveFriendConfirm: "Tem certeza que deseja remover ",
                alertFriendRemoved: " foi removido(a) da sua lista de amigos.",
                alertSelectFriendChat: "Selecione um amigo para conversar.",
                alertGroupExists: "Já existe um grupo com este nome. Escolha outro.",
                alertGroupNameRequired: "Por favor, digite um nome para o grupo.",
                alertGroupMinMembers: "Um grupo precisa de pelo menos você e mais um membro.",
                alertGroupCreated: " criado com sucesso!",
                alertGroupNotFound: "Grupo não encontrado.",
                alertMessageNotFound: "Mensagem não encontrada.",
                alertDeleteMessageConfirm: "Tem certeza que deseja deletar esta mensagem?",
                alertAdminDeleteUserConfirm: "Excluir usuário ",
                alertAdminChangePassword: "Digite nova senha para ",
                alertAdminPasswordChanged: " senha alterada com sucesso!",
                alertDataUpdated: "Dados atualizados com sucesso!",
                alertAccountDeleted: "Conta excluída com sucesso.",
                // Translations for profile view
                profileViewTitle: "Perfil de ",
                profileViewSendRequest: "Enviar Solicitação de Amizade",
                profileViewRemoveFriend: "Remover Amigo",
                profileViewPendingRequest: "Solicitação Pendente",
                profileViewAcceptRequest: "Aceitar Solicitação",
                profileViewRejectRequest: "Rejeitar Solicitação",
                profileViewChat: "Conversar",
                // NEW: Status translations
                statusSectionTitle: "Meus Status e de Amigos",
                createStatusTitle: "Criar Novo Status",
                statusInputPlaceholder: "O que você está pensando?",
                postStatusBtn: "POSTAR STATUS",
                myStatusTitle: "Meu Status Atual",
                friendStatusesTitle: "Status dos Amigos",
                noStatusYet: "Você não tem nenhum status publicado.",
                noFriendStatuses: "Nenhum amigo publicou status ainda.",
                editStatusTitle: "Editar Status",
                alertStatusSomething: "Digite algo ou escolha uma imagem para o status!",
                alertStatusNotFound: "Status não encontrado.",
                alertDeleteStatusConfirm: "Tem certeza que deseja excluir seu status?",
                alertStatusRemoved: "Status removido com sucesso.",
                alertStatusUpdated: "Status atualizado com sucesso."
            },
            es: {
                darkModeBtn: "Cambiar Modo",
                loginTitle: "Iniciar Sesión",
                statusNotLogged: "Estado: No logueado",
                statusLoggedAs: "Logueado como",
                uploadProfilePictureBtn: "Subir/Cambiar Foto",
                removeProfilePictureBtn: "Eliminar Foto",
                usernamePlaceholder: "Nombre de usuario",
                passwordPlaceholder: "Contraseña",
                bioPlaceholder: "Tu biografía (opcional)",
                editUserBtn: "Editar nombre y contraseña",
                editBioBtn: "Guardar Biografía",
                deleteAccountBtn: "Eliminar cuenta",
                registerBtn: "Registrarse",
                loginBtn: "Entrar",
                logoutBtn: "Salir",
                adminPanelTitle: "Administrar Usuarios (Admin)",
                adminDeleteBtn: "Eliminar",
                adminEditBtn: "Editar Contraseña",
                friendsSectionTitle: "Administrar Amigos",
                addFriendInput: "Nombre de usuario para solicitar",
                sendFriendRequestButton: "Enviar Solicitud",
                myFriendsTitle: "Mis Amigos",
                removeFriendBtn: "Eliminar",
                sentRequestsTitle: "Solicitudes Enviadas",
                receivedRequestsTitle: "Solicitudes Recibidas",
                acceptBtn: "Aceptar",
                rejectBtn: "Rechazar",
                chatSectionTitle: "Chatear con Amigos",
                noFriendsChat: "Ningún amigo para chatear.",
                chatHeader: "Conversación con",
                messageInputPlaceholder: "Escribe tu mensaje...",
                sendBtn: "Enviar",
                noMessagesYet: "Aún no hay mensajes. ¡Empieza a chatear!",
                groupSectionTitle: "Administrar Grupos",
                createGroupTitle: "Crear Nuevo Grupo",
                groupNameInput: "Nombre del Grupo",
                selectMembersText: "Seleccionar Miembros:",
                createGroupBtn: "Crear Grupo",
                myGroupsTitle: "Mis Grupos",
                noGroupsYet: "Aún no eres parte de ningún grupo.",
                groupChatHeader: "Chat del Grupo:",
                groupMessageInputPlaceholder: "Escribe tu mensaje en el grupo...",
                postsTitle: "Publicaciones",
                tweetInputPlaceholder: "¿Qué está pasando?",
                postTweetBtn: "PUBLICAR",
                searchPlaceholder: "Buscar publicaciones...",
                editPostTitle: "Editar Publicación",
                cancelBtn: "Cancelar",
                saveBtn: "Guardar",
                editReplyTitle: "Editar Respuesta",
                editChatMessageTitle: "Editar Mensaje",
                you: "Tú",
                editBtn: "Editar",
                deleteBtn: "Eliminar",
                replyBtn: "Responder",
                replyInputPlaceholder: "Escribe tu respuesta...",
                languageLabel: "Idioma:",
                alertLoginRequired: "Necesitas iniciar sesión para ",
                alertFillFields: "¡Rellena nombre de usuario y contraseña!",
                alertUserExists: "¡El usuario ya existe!",
                alertRegistrationComplete: "¡Registro completado! Ahora inicia sesión.",
                alertWelcome: "¡Bienvenido, ",
                alertInvalidCredentials: "Usuario o contraseña incorrectos.",
                alertPostSomething: "¡Escribe algo o elige una imagen para publicar!",
                alertPostNotFound: "Publicación no encontrada.",
                alertDeletePostConfirm: "¿Eliminar esta publicación?",
                alertReplySomething: "¡Escribe algo o elige una imagen para responder!",
                alertReplyNotFound: "Respuesta no encontrada.",
                alertDeleteReplyConfirm: "¿Eliminar esta respuesta?",
                alertEmptyContent: "Contenido vacío no permitido.",
                alertSendRequestSelf: "¡No puedes enviarte una solicitud de amistad a ti mismo!",
                alertUserNotExists: "El usuario no existe.",
                alertAlreadyFriend: " ya es tu amigo.",
                alertRequestSentAlready: "Ya enviaste una solicitud de amistad a ",
                alertRequestReceivedAlready: "Ya recibiste una solicitud de amistad de ",
                alertFriendRequestSent: "¡Solicitud de amistad enviada a ",
                alertAcceptFriendRequest: "Aceptaste la solicitud de amistad de ",
                alertNowFriends: ". ¡Ahora son amigos!",
                alertRejectFriendRequestConfirm: "¿Estás seguro de que quieres rechazar la solicitud de ",
                alertRejectedFriendRequest: "Rechazaste la solicitud de amistad de ",
                alertRemoveFriendConfirm: "¿Estás seguro de que quieres eliminar a ",
                alertFriendRemoved: " ha sido eliminado(a) de tu lista de amigos.",
                alertSelectFriendChat: "Selecciona un amigo para chatear.",
                alertGroupExists: "Ya existe un grupo con este nombre. Elige otro.",
                alertGroupNameRequired: "Por favor, introduce un nombre para el grupo.",
                alertGroupMinMembers: "Un grupo necesita al menos tú y un miembro más.",
                alertGroupCreated: " creado exitosamente!",
                alertGroupNotFound: "Grupo no encontrado.",
                alertMessageNotFound: "Mensaje no encontrado.",
                alertDeleteMessageConfirm: "¿Estás seguro de que quieres eliminar este mensaje?",
                alertAdminDeleteUserConfirm: "¿Eliminar usuario ",
                alertAdminChangePassword: "Introduce nueva contraseña para ",
                alertAdminPasswordChanged: " contraseña cambiada con éxito!",
                alertDataUpdated: "¡Datos actualizados con éxito!",
                alertAccountDeleted: "Cuenta eliminada exitosamente.",
                // Translations for profile view
                profileViewTitle: "Perfil de ",
                profileViewSendRequest: "Enviar Solicitud de Amistad",
                profileViewRemoveFriend: "Eliminar Amigo",
                profileViewPendingRequest: "Solicitud Pendiente",
                profileViewAcceptRequest: "Aceptar Solicitud",
                profileViewRejectRequest: "Rechazar Solicitud",
                profileViewChat: "Chatear",
                // NEW: Status translations
                statusSectionTitle: "Mis Estados y de Amigos",
                createStatusTitle: "Crear Nuevo Estado",
                statusInputPlaceholder: "¿Qué estás pensando?",
                postStatusBtn: "PUBLICAR ESTADO",
                myStatusTitle: "Mi Estado Actual",
                friendStatusesTitle: "Estados de los Amigos",
                noStatusYet: "No tienes ningún estado publicado.",
                noFriendStatuses: "Ningún amigo ha publicado estados todavía.",
                editStatusTitle: "Editar Estado",
                alertStatusSomething: "¡Escribe algo o elige una imagen para el estado!",
                alertStatusNotFound: "Estado no encontrado.",
                alertDeleteStatusConfirm: "¿Estás seguro de que quieres eliminar tu estado?",
                alertStatusRemoved: "Estado eliminado exitosamente.",
                alertStatusUpdated: "Estado actualizado exitosamente."
            },
            en: {
                darkModeBtn: "Toggle Mode",
                loginTitle: "Login",
                statusNotLogged: "Status: Not logged in",
                statusLoggedAs: "Logged in as",
                uploadProfilePictureBtn: "Upload/Change Photo",
                removeProfilePictureBtn: "Remove Photo",
                usernamePlaceholder: "Username",
                passwordPlaceholder: "Password",
                bioPlaceholder: "Your bio (optional)",
                editUserBtn: "Edit username and password",
                editBioBtn: "Save Bio",
                deleteAccountBtn: "Delete account",
                registerBtn: "Register",
                loginBtn: "Login",
                logoutBtn: "Logout",
                adminPanelTitle: "Manage Users (Admin)",
                adminDeleteBtn: "Delete",
                adminEditBtn: "Edit Password",
                friendsSectionTitle: "Manage Friends",
                addFriendInput: "Username to send request",
                sendFriendRequestButton: "Send Request",
                myFriendsTitle: "My Friends",
                removeFriendBtn: "Remove",
                sentRequestsTitle: "Sent Requests",
                receivedRequestsTitle: "Received Requests",
                acceptBtn: "Accept",
                rejectBtn: "Reject",
                chatSectionTitle: "Chat with Friends",
                noFriendsChat: "No friends to chat with.",
                chatHeader: "Chat with",
                messageInputPlaceholder: "Type your message...",
                sendBtn: "Send",
                noMessagesYet: "No messages yet. Start chatting!",
                groupSectionTitle: "Manage Groups",
                createGroupTitle: "Create New Group",
                groupNameInput: "Group Name",
                selectMembersText: "Select Members:",
                createGroupBtn: "Create Group",
                myGroupsTitle: "My Groups",
                noGroupsYet: "You are not part of any group yet.",
                groupChatHeader: "Group Chat:",
                groupMessageInputPlaceholder: "Type your message in the group...",
                postsTitle: "Posts",
                tweetInputPlaceholder: "What's happening?",
                postTweetBtn: "POST",
                searchPlaceholder: "Search posts...",
                editPostTitle: "Edit Post",
                cancelBtn: "Cancel",
                saveBtn: "Save",
                editReplyTitle: "Edit Reply",
                editChatMessageTitle: "Edit Message",
                you: "You",
                editBtn: "Edit",
                deleteBtn: "Delete",
                replyBtn: "Reply",
                replyInputPlaceholder: "Write your reply...",
                languageLabel: "Language:",
                alertLoginRequired: "You need to be logged in to ",
                alertFillFields: "Fill in username and password!",
                alertUserExists: "User already exists!",
                alertRegistrationComplete: "Registration complete! Now log in.",
                alertWelcome: "Welcome, ",
                alertInvalidCredentials: "Incorrect username or password.",
                alertPostSomething: "Type something or choose an image to post!",
                alertPostNotFound: "Post not found.",
                alertDeletePostConfirm: "Delete this post?",
                alertReplySomething: "Type something or choose an image to reply!",
                alertReplyNotFound: "Reply not found.",
                alertDeleteReplyConfirm: "Delete this reply?",
                alertEmptyContent: "Empty content not allowed.",
                alertSendRequestSelf: "You cannot send a friend request to yourself!",
                alertUserNotExists: "The user does not exist.",
                alertAlreadyFriend: " is already your friend.",
                alertRequestSentAlready: "You have already sent a friend request to ",
                alertRequestReceivedAlready: "You have already received a friend request from ",
                alertFriendRequestSent: "Friend request sent to ",
                alertAcceptFriendRequest: "You accepted the friend request from ",
                alertNowFriends: ". You are now friends!",
                alertRejectFriendRequestConfirm: "Are you sure you want to reject the request from ",
                alertRejectedFriendRequest: "You rejected the friend request from ",
                alertRemoveFriendConfirm: "Are you sure you want to remove ",
                alertFriendRemoved: " has been removed from your friends list.",
                alertSelectFriendChat: "Select a friend to chat.",
                alertGroupExists: "A group with this name already exists. Choose another.",
                alertGroupNameRequired: "Please enter a name for the group.",
                alertGroupMinMembers: "A group needs at least you and one more member.",
                alertGroupCreated: " created successfully!",
                alertGroupNotFound: "Group not found.",
                alertMessageNotFound: "Message not found.",
                alertDeleteMessageConfirm: "Are you sure you want to delete this message?",
                alertAdminDeleteUserConfirm: "Delete user ",
                alertAdminChangePassword: "Enter new password for ",
                alertAdminPasswordChanged: " password changed successfully!",
                alertDataUpdated: "Data updated successfully!",
                alertAccountDeleted: "Account deleted successfully.",
                // Translations for profile view
                profileViewTitle: "Profile of ",
                profileViewSendRequest: "Send Friend Request",
                profileViewRemoveFriend: "Remove Friend",
                profileViewPendingRequest: "Pending Request",
                profileViewAcceptRequest: "Accept Request",
                profileViewRejectRequest: "Reject Request",
                profileViewChat: "Chat",
                // NEW: Status translations
                statusSectionTitle: "My & Friends' Statuses",
                createStatusTitle: "Create New Status",
                statusInputPlaceholder: "What's on your mind?",
                postStatusBtn: "POST STATUS",
                myStatusTitle: "My Current Status",
                friendStatusesTitle: "Friends' Statuses",
                noStatusYet: "You haven't posted any status yet.",
                noFriendStatuses: "No friends have posted statuses yet.",
                editStatusTitle: "Edit Status",
                alertStatusSomething: "Type something or choose an image for the status!",
                alertStatusNotFound: "Status not found.",
                alertDeleteStatusConfirm: "Are you sure you want to delete your status?",
                alertStatusRemoved: "Status removed successfully.",
                alertStatusUpdated: "Status updated successfully."
            }
        };

        let currentLanguage = localStorage.getItem("language") || "pt";

        function setLanguage(lang) {
            currentLanguage = lang;
            localStorage.setItem("language", lang);

            document.getElementById("languageSelect").value = lang;

            document.querySelector(".mode-btn").textContent = translations[lang].darkModeBtn;
            document.getElementById("loginTitle").textContent = translations[lang].loginTitle;
            document.getElementById("uploadProfilePictureBtn").textContent = translations[lang].uploadProfilePictureBtn;
            document.getElementById("removeProfilePictureBtn").textContent = translations[lang].removeProfilePictureBtn;
            document.getElementById("usernameInput").placeholder = translations[lang].usernamePlaceholder;
            document.getElementById("passwordInput").placeholder = translations[lang].passwordPlaceholder;
            document.getElementById("bioInput").placeholder = translations[lang].bioPlaceholder;
            document.getElementById("editUserBtn").textContent = translations[lang].editUserBtn;
            document.getElementById("editBioBtn").textContent = translations[lang].editBioBtn;
            document.getElementById("deleteAccountBtn").textContent = translations[lang].deleteAccountBtn;
            document.getElementById("registerBtn").textContent = translations[lang].registerBtn;
            document.getElementById("loginBtn").textContent = translations[lang].loginBtn;
            document.getElementById("logoutBtn").textContent = translations[lang].logoutBtn;
            document.getElementById("adminPanelTitle").textContent = translations[lang].adminPanelTitle;
            document.getElementById("friendsSectionTitle").textContent = translations[lang].friendsSectionTitle;
            document.getElementById("addFriendInput").placeholder = translations[lang].addFriendInput;
            document.getElementById("sendFriendRequestButton").textContent = translations[lang].sendFriendRequestButton;
            document.getElementById("myFriendsTitle").textContent = translations[lang].myFriendsTitle;
            document.getElementById("sentRequestsTitle").textContent = translations[lang].sentRequestsTitle;
            document.getElementById("receivedRequestsTitle").textContent = translations[lang].receivedRequestsTitle;
            document.getElementById("chatSectionTitle").textContent = translations[lang].chatSectionTitle;
            document.getElementById("groupSectionTitle").textContent = translations[lang].groupSectionTitle;
            document.getElementById("createGroupTitle").textContent = translations[lang].createGroupTitle;
            document.getElementById("groupNameInput").placeholder = translations[lang].groupNameInput;
            document.getElementById("selectMembersText").textContent = translations[lang].selectMembersText;
            document.getElementById("createGroupBtn").textContent = translations[lang].createGroupBtn;
            document.getElementById("myGroupsTitle").textContent = translations[lang].myGroupsTitle;
            document.getElementById("postsTitle").textContent = translations[lang].postsTitle;
            document.getElementById("tweetInput").placeholder = translations[lang].tweetInputPlaceholder;
            document.getElementById("postTweetBtn").textContent = translations[lang].postTweetBtn;
            document.getElementById("searchBar").placeholder = translations[lang].searchPlaceholder;
            document.getElementById("chatMessageInput").placeholder = translations[lang].messageInputPlaceholder;
            document.getElementById("sendChatMessageBtn").textContent = translations[lang].sendBtn;
            document.getElementById("groupChatMessageInput").placeholder = translations[lang].groupMessageInputPlaceholder;
            document.getElementById("sendGroupChatMessageBtn").textContent = translations[lang].sendBtn;
            document.getElementById("editPostTitle").textContent = translations[lang].editPostTitle;
            document.getElementById("cancelEditPostBtn").textContent = translations[lang].cancelBtn;
            document.getElementById("saveEditPostBtn").textContent = translations[lang].saveBtn;
            document.getElementById("editReplyTitle").textContent = translations[lang].editReplyTitle;
            document.getElementById("cancelEditReplyBtn").textContent = translations[lang].cancelBtn;
            document.getElementById("saveEditReplyBtn").textContent = translations[lang].saveBtn;
            document.getElementById("editChatMessageTitle").textContent = translations[lang].editChatMessageTitle;
            document.getElementById("cancelEditChatMessageBtn").textContent = translations[lang].cancelBtn;
            document.getElementById("saveEditChatMessageBtn").textContent = translations[lang].saveBtn;
            document.getElementById("languageLabel").textContent = translations[lang].languageLabel;
            // NEW: Status translations update
            document.getElementById("statusSectionTitle").textContent = translations[lang].statusSectionTitle;
            document.getElementById("createStatusTitle").textContent = translations[lang].createStatusTitle;
            document.getElementById("statusTextInput").placeholder = translations[lang].statusInputPlaceholder;
            document.getElementById("postStatusBtn").textContent = translations[lang].postStatusBtn;
            document.getElementById("myStatusTitle").textContent = translations[lang].myStatusTitle;
            document.getElementById("friendStatusesTitle").textContent = translations[lang].friendStatusesTitle;
            document.getElementById("editStatusTitle").textContent = translations[lang].editStatusTitle;
            document.getElementById("cancelEditStatusBtn").textContent = translations[lang].cancelBtn;
            document.getElementById("saveEditStatusBtn").textContent = translations[lang].saveBtn;


            updateStatus();
            loadTweets();
            showAdminPanel();
            loadFriendStatus();
            loadChatFriendsList();
            loadGroupMembersForCreation();
            loadMyGroups();
            if (currentChatFriend) {
                startChat(currentChatFriend);
            }
            if (currentGroupChatId) {
                startGroupChat(currentGroupChatId);
            }
            loadStatuses(); // NEW: Load statuses on language change
        }


        function getAllUsers() {
            let users = JSON.parse(localStorage.getItem("users")) || {};
            for (const username in users) {
                if (!users[username].friends) users[username].friends = [];
                if (!users[username].sentRequests) users[username].sentRequests = [];
                if (!users[username].receivedRequests) users[username].receivedRequests = [];
                if (!users[username].profilePicture) users[username].profilePicture = null;
                if (!users[username].bio) users[username].bio = "";
                // NEW: Initialize status for existing users
                if (!users[username].status) users[username].status = null;
            }
            return users;
        }

        function getAllMessages() {
            return JSON.parse(localStorage.getItem("chatMessages")) || {};
        }

        function getAllGroups() {
            let groups = JSON.parse(localStorage.getItem("groups")) || [];
            groups.forEach(group => {
                if (!group.messages) group.messages = [];
            });
            return groups;
        }

        (function ensureAdmin() {
            let users = getAllUsers();
            if (!users["admin"]) {
                users["admin"] = { password: "admin", friends: [], sentRequests: [], receivedRequests: [], profilePicture: null, bio: "", status: null };
                localStorage.setItem("users", JSON.stringify(users));
            }
        })();

        function toggleMode() {
            document.body.classList.toggle("dark-mode");
            localStorage.setItem("mode", document.body.classList.contains("dark-mode") ? "dark" : "light");
            document.getElementById("editModalContent").classList.toggle("dark-mode", document.body.classList.contains("dark-mode"));
            document.getElementById("editReplyModalContent").classList.toggle("dark-mode", document.body.classList.contains("dark-mode"));
            document.getElementById("editChatMessageModalContent").classList.toggle("dark-mode", document.body.classList.contains("dark-mode"));
            // Toggle dark mode for profile view
            document.querySelector('#userProfileView .profile-details').classList.toggle('dark-mode', document.body.classList.contains('dark-mode'));
            // NEW: Toggle dark mode for status modal
            document.getElementById("editStatusModalContent").classList.toggle("dark-mode", document.body.classList.contains("dark-mode"));
        }

        function applyMode() {
            if (localStorage.getItem("mode") === "dark") {
                document.body.classList.add("dark-mode");
                document.getElementById("editModalContent").classList.add("dark-mode");
                document.getElementById("editReplyModalContent").classList.add("dark-mode");
                document.getElementById("editChatMessageModalContent").classList.add("dark-mode");
                // Apply dark mode for profile view on load
                document.querySelector('#userProfileView .profile-details').classList.add('dark-mode');
                // NEW: Apply dark mode for status modal on load
                document.getElementById("editStatusModalContent").classList.add("dark-mode");
            }
        }

        function updateStatus() {
            const statusText = document.getElementById("statusText");
            const sidebarProfilePicture = document.getElementById("sidebarProfilePicture");
            const profilePictureInput = document.getElementById("profilePictureInput");
            const uploadProfilePictureBtn = document.getElementById("uploadProfilePictureBtn");
            const removeProfilePictureBtn = document.getElementById("removeProfilePictureBtn");
            const bioInput = document.getElementById("bioInput");
            const userBioDisplay = document.getElementById("userBio");
            const editBioBtn = document.getElementById("editBioBtn");


            if (currentUser) {
                statusText.textContent = `${translations[currentLanguage].statusLoggedAs} ${currentUser}`;
                const users = getAllUsers();
                const user = users[currentUser];
                if (user) {
                    if (user.profilePicture) {
                        sidebarProfilePicture.src = user.profilePicture;
                    } else {
                        sidebarProfilePicture.src = "default-profile.png";
                    }
                    uploadProfilePictureBtn.style.display = "block";
                    removeProfilePictureBtn.style.display = "block";

                    userBioDisplay.textContent = user.bio || "";
                    bioInput.value = user.bio || "";
                    bioInput.style.display = "block";
                    editBioBtn.style.display = "block";
                }
            } else {
                statusText.textContent = translations[currentLanguage].statusNotLogged;
                sidebarProfilePicture.src = "default-profile.png";
                uploadProfilePictureBtn.style.display = "none";
                removeProfilePictureBtn.style.display = "none";
                bioInput.style.display = "none";
                userBioDisplay.textContent = "";
                editBioBtn.style.display = "none";
            }
            showAdminPanel();
            loadFriendStatus();
            loadChatFriendsList();
            loadGroupSection();
            document.getElementById("chatSection").style.display = currentUser ? "block" : "none";
            document.getElementById("groupSection").style.display = currentUser ? "block" : "none";
            // NEW: Show/hide status section based on login
            document.getElementById("statusSection").style.display = currentUser ? "block" : "none";

            closeChat();
            closeGroupChat();
            closeUserProfileView(); // Close profile view on status change
            loadStatuses(); // NEW: Load statuses after login/logout
        }

        function saveBio() {
            if (!currentUser) {
                alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].editBioBtn.toLowerCase() + "!");
                return;
            }
            const bioText = document.getElementById("bioInput").value;
            let users = getAllUsers();
            if (users[currentUser]) {
                users[currentUser].bio = bioText;
                localStorage.setItem("users", JSON.stringify(users));
                alert(translations[currentLanguage].alertDataUpdated);
                updateStatus();
                loadTweets(); // Reload posts to update bio there
                loadStatuses(); // Reload statuses to update bio there
            }
        }


        function triggerProfilePictureInput() {
            if (!currentUser) {
                alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].uploadProfilePictureBtn.toLowerCase() + "!");
                return;
            }
            document.getElementById("profilePictureInput").click();
        }

        document.getElementById("profilePictureInput").addEventListener("change", function(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                const profilePictureData = e.target.result;
                let users = getAllUsers();
                if (users[currentUser]) {
                    users[currentUser].profilePicture = profilePictureData;
                    localStorage.setItem("users", JSON.stringify(users));
                    updateStatus();
                    loadTweets();
                    loadChatMessages(currentChatFriend);
                    loadGroupMessages(currentGroupChatId);
                    loadStatuses(); // NEW: Update statuses with new profile pic
                }
            };
            reader.readAsDataURL(file);
        });

        function removeProfilePicture() {
            if (!currentUser) {
                alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].removeProfilePictureBtn.toLowerCase() + "!");
                return;
            }
            if (!confirm(translations[currentLanguage].alertRemoveFriendConfirm + translations[currentLanguage].you + translations[currentLanguage].removeProfilePictureBtn.toLowerCase() + "?")) return;

            let users = getAllUsers();
            if (users[currentUser]) {
                users[currentUser].profilePicture = null;
                localStorage.setItem("users", JSON.stringify(users));
                updateStatus();
                loadTweets();
                loadChatMessages(currentChatFriend);
                loadGroupMessages(currentGroupChatId);
                loadStatuses(); // NEW: Update statuses with default profile pic
            }
        }


        function register() {
            const username = document.getElementById("usernameInput").value.trim();
            const password = document.getElementById("passwordInput").value.trim();
            const bio = document.getElementById("bioInput").value.trim();
            let users = getAllUsers();
            if (!username || !password) return alert(translations[currentLanguage].alertFillFields);
            if (users[username]) return alert(translations[currentLanguage].alertUserExists);
            users[username] = { password, friends: [], sentRequests: [], receivedRequests: [], profilePicture: null, bio: bio, status: null }; // NEW: Initialize status
            localStorage.setItem("users", JSON.stringify(users));
            alert(translations[currentLanguage].alertRegistrationComplete);
            document.getElementById("bioInput").value = "";
        }

        function login() {
            const username = document.getElementById("usernameInput").value.trim();
            const password = document.getElementById("passwordInput").value.trim();
            let users = getAllUsers();

            if (users[username] && users[username].password === password) {
                currentUser = username;
                localStorage.setItem("currentUser", JSON.stringify(currentUser));
                alert(`${translations[currentLanguage].alertWelcome}${username}!`);
                updateStatus();
                loadTweets();
                showAdminPanel();
                loadFriendStatus();
                loadChatFriendsList();
                loadGroupSection();
                document.getElementById("chatSection").style.display = "block";
                document.getElementById("groupSection").style.display = "block";
                document.getElementById("statusSection").style.display = "block"; // NEW: Show status section on login
            } else {
                alert(translations[currentLanguage].alertInvalidCredentials);
            }
        }

        function logout() {
            currentUser = null;
            localStorage.removeItem("currentUser");
            updateStatus();
            loadTweets();
            showAdminPanel();
            loadFriendStatus();
            loadChatFriendsList();
            loadGroupSection();
            document.getElementById("chatSection").style.display = "none";
            document.getElementById("groupSection").style.display = "none";
            document.getElementById("statusSection").style.display = "none"; // NEW: Hide status section on logout
        }

        function editUser() {
            if (!currentUser) return alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].editUserBtn.toLowerCase() + "!");
            const newUsername = prompt(`${translations[currentLanguage].usernamePlaceholder} (${translations[currentLanguage].alertEmptyContent}):`, currentUser);
            const newPassword = prompt(`${translations[currentLanguage].passwordPlaceholder} (${translations[currentLanguage].alertEmptyContent}):`, "");
            let users = getAllUsers();

            if (newUsername && newUsername !== currentUser) {
                if (users[newUsername]) return alert(translations[currentLanguage].alertUserExists);

                users[newUsername] = { ...users[currentUser] };

                let posts = JSON.parse(localStorage.getItem("tweets")) || [];
                posts.forEach(post => {
                    if (post.author === currentUser) post.author = newUsername;
                    if (post.replies) {
                        post.replies.forEach(reply => {
                            if (reply.author === currentUser) reply.author = newUsername;
                        });
                    }
                });
                localStorage.setItem("tweets", JSON.stringify(posts));

                Object.keys(users).forEach(userKey => {
                    if (userKey !== newUsername) {
                        if (users[userKey].friends) {
                            users[userKey].friends = users[userKey].friends.map(friend => friend === currentUser ? newUsername : friend);
                        }
                        if (users[userKey].sentRequests) {
                            users[userKey].sentRequests = users[userKey].sentRequests.map(req => req === currentUser ? newUsername : req);
                        }
                        if (users[userKey].receivedRequests) {
                            users[userKey].receivedRequests = users[userKey].receivedRequests.map(req => req === currentUser ? newUsername : req);
                        }
                    }
                });

                let messages = getAllMessages();
                const updatedMessages = {};
                for (const convoId in messages) {
                    const [user1, user2] = convoId.split('-');
                    let newConvoId = convoId;
                    if (user1 === currentUser) newConvoId = `${newUsername}-${user2}`;
                    if (user2 === currentUser) newConvoId = `${user1}-${newUsername}`;
                    newConvoId = newConvoId.split('-').sort().join('-');

                    updatedMessages[newConvoId] = messages[convoId].map(msg => {
                        if (msg.sender === currentUser) msg.sender = newUsername;
                        return msg;
                    });
                }
                localStorage.setItem("chatMessages", JSON.stringify(updatedMessages));

                let groups = getAllGroups();
                groups.forEach(group => {
                    if (group.creator === currentUser) group.creator = newUsername;
                    group.members = group.members.map(member => member === currentUser ? newUsername : member);
                    group.messages.forEach(msg => {
                        if (msg.sender === currentUser) msg.sender = newUsername;
                    });
                });
                localStorage.setItem("groups", JSON.stringify(groups));


                delete users[currentUser];
                currentUser = newUsername;
            }
            if (newPassword) {
                users[currentUser].password = newPassword;
            }
            localStorage.setItem("users", JSON.stringify(users));
            localStorage.setItem("currentUser", JSON.stringify(currentUser));
            alert(translations[currentLanguage].alertDataUpdated);
            updateStatus();
            loadTweets();
            showAdminPanel();
            loadFriendStatus();
            loadGroupSection();
            loadStatuses(); // NEW: Reload statuses after username change
        }

        function deleteAccount() {
            if (!currentUser) return alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].deleteAccountBtn.toLowerCase() + "!");
            if (!confirm(translations[currentLanguage].alertAccountDeleted)) return;
            let users = getAllUsers();
            delete users[currentUser];
            localStorage.setItem("users", JSON.stringify(users));

            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            posts = posts.filter(post => post.author !== currentUser);
            posts.forEach(post => {
                if (post.replies) {
                    post.replies = post.replies.filter(reply => reply.author !== currentUser);
                }
            });
            localStorage.setItem("tweets", JSON.stringify(posts));

            Object.keys(users).forEach(userKey => {
                if (users[userKey].friends) {
                    users[userKey].friends = users[userKey].friends.filter(friend => friend !== currentUser);
                }
                if (users[userKey].sentRequests) {
                    users[userKey].sentRequests = users[userKey].sentRequests.filter(req => req !== currentUser);
                }
                if (users[userKey].receivedRequests) {
                    users[userKey].receivedRequests = users[userKey].receivedRequests.filter(req => req !== currentUser);
                }
            });

            let messages = getAllMessages();
            const filteredMessages = {};
            for (const convoId in messages) {
                const [user1, user2] = convoId.split('-');
                if (user1 !== currentUser && user2 !== currentUser) {
                    filteredMessages[convoId] = messages[convoId].filter(msg => msg.sender !== currentUser);
                }
            }
            localStorage.setItem("chatMessages", JSON.stringify(filteredMessages));

            let groups = getAllGroups();
            groups.forEach(group => {
                group.members = group.members.filter(member => member !== currentUser);
                group.messages = group.messages.filter(msg => msg.sender !== currentUser);
            });
            groups = groups.filter(group => group.members.length > 0);
            localStorage.setItem("groups", JSON.stringify(groups));

            currentUser = null;
            localStorage.removeItem("currentUser");
            alert(translations[currentLanguage].alertAccountDeleted);
            updateStatus();
            loadTweets();
            showAdminPanel();
            loadFriendStatus();
            loadGroupSection();
            loadStatuses(); // NEW: Reload statuses after account deletion
        }

        function showAdminPanel() {
            const panel = document.getElementById("adminPanel");
            const userListDiv = document.getElementById("userList");
            if (currentUser === "admin") {
                panel.style.display = "block";
                let users = getAllUsers();
                userListDiv.innerHTML = "";
                Object.keys(users).forEach(user => {
                    const div = document.createElement("div");
                    div.className = "admin-user";
                    div.innerHTML = `<span>${user}</span>`;
                    if (user !== "admin") {
                        const btnDel = document.createElement("button");
                        btnDel.className = "delete-btn";
                        btnDel.textContent = translations[currentLanguage].adminDeleteBtn;
                        btnDel.onclick = () => {
                            if (confirm(`${translations[currentLanguage].alertAdminDeleteUserConfirm}${user}?`)) {
                                deleteUser(user);
                            }
                        };
                        const btnEdit = document.createElement("button");
                        btnEdit.className = "edit-btn";
                        btnEdit.textContent = translations[currentLanguage].adminEditBtn;
                        btnEdit.onclick = () => {
                            let newPass = prompt(`${translations[currentLanguage].alertAdminChangePassword}${user}:`);
                            if (newPass) changeUserPassword(user, newPass);
                        };
                        div.appendChild(btnEdit);
                        div.appendChild(btnDel);
                    }
                    userListDiv.appendChild(div);
                });
            } else {
                panel.style.display = "none";
            }
        }

        function deleteUser(username) {
            let users = getAllUsers();
            delete users[username];
            localStorage.setItem("users", JSON.stringify(users));

            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            posts = posts.filter(p => p.author !== username);
            posts.forEach(post => {
                if (post.replies) {
                    post.replies = post.replies.filter(reply => reply.author !== username);
                }
            });
            localStorage.setItem("tweets", JSON.stringify(posts));

            Object.keys(users).forEach(userKey => {
                if (users[userKey].friends) {
                    users[userKey].friends = users[userKey].friends.filter(friend => friend !== username);
                }
                if (users[userKey].sentRequests) {
                    users[userKey].sentRequests = users[userKey].sentRequests.filter(req => req !== username);
                }
                if (users[userKey].receivedRequests) {
                    users[userKey].receivedRequests = users[userKey].receivedRequests.filter(req => req !== username);
                }
            });

            let messages = getAllMessages();
            const filteredMessages = {};
            for (const convoId in messages) {
                const [user1, user2] = convoId.split('-');
                if (user1 !== username && user2 !== username) {
                    filteredMessages[convoId] = messages[convoId].filter(msg => msg.sender !== username);
                }
            }
            localStorage.setItem("chatMessages", JSON.stringify(filteredMessages));

            let groups = getAllGroups();
            groups.forEach(group => {
                group.members = group.members.filter(member => member !== username);
                group.messages = group.messages.filter(msg => msg.sender !== username);
            });
            groups = groups.filter(group => group.members.length > 0);
            localStorage.setItem("groups", JSON.stringify(groups));

            loadTweets();
            showAdminPanel();
            loadFriendStatus();
            loadGroupSection();
            closeUserProfileView(); // Close profile view if the user being viewed is deleted
            loadStatuses(); // NEW: Reload statuses after user deletion
        }

        function changeUserPassword(username, newPass) {
            let users = getAllUsers();
            if (users[username]) {
                users[username].password = newPass;
                localStorage.setItem("users", JSON.stringify(users));
                alert(`${translations[currentLanguage].alertAdminPasswordChanged}`);
            }
        }

        function postTweet() {
            if (!currentUser) return alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].postTweetBtn.toLowerCase() + "!");
            const text = document.getElementById("tweetInput").value.trim();
            const imageInput = document.getElementById("imageInput");
            if (!text && !imageInput.files.length) return alert(translations[currentLanguage].alertPostSomething);

            const reader = new FileReader();
            if (imageInput.files.length) {
                reader.onload = function(e) {
                    saveTweet(text, e.target.result);
                };
                reader.readAsDataURL(imageInput.files[0]);
            } else {
                saveTweet(text, null);
            }
        }

        function saveTweet(text, imageData) {
            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            posts.push({
                id: Date.now(),
                author: currentUser,
                content: text,
                image: imageData,
                replies: []
            });
            localStorage.setItem("tweets", JSON.stringify(posts));
            document.getElementById("tweetInput").value = "";
            document.getElementById("imageInput").value = "";
            loadTweets();
        }

        function loadTweets() {
            const container = document.getElementById("tweets");
            container.innerHTML = "";
            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            posts.sort((a, b) => b.id - a.id);

            const searchTerm = document.getElementById("searchBar").value.toLowerCase();
            const filteredPosts = posts.filter(post => {
                const contentMatch = post.content.toLowerCase().includes(searchTerm);
                const authorMatch = post.author.toLowerCase().includes(searchTerm);
                const repliesMatch = post.replies.some(reply =>
                    reply.content.toLowerCase().includes(searchTerm) ||
                    reply.author.toLowerCase().includes(searchTerm)
                );
                return contentMatch || authorMatch || repliesMatch;
            });


            filteredPosts.forEach(post => {
                container.appendChild(createPostElement(post));
            });
        }

        function createPostElement(post) {
            const div = document.createElement("div");
            div.className = "post";
            div.id = `post-${post.id}`;

            const users = getAllUsers();
            const authorProfilePic = users[post.author] ? users[post.author].profilePicture : 'default-profile.png';
            const authorBio = users[post.author] ? users[post.author].bio : '';

            const userInfoDiv = document.createElement("div");
            userInfoDiv.className = "user-info";
            // Add event listeners to the image and username
            userInfoDiv.innerHTML = `
                <img src="${authorProfilePic}" alt="Foto de Perfil" class="profile-pic-small" onclick="viewUserProfile('${post.author}')" />
                <strong onclick="viewUserProfile('${post.author}')">${post.author}</strong>
            `;
            div.appendChild(userInfoDiv);

            if (authorBio) {
                const bioParagraph = document.createElement("p");
                bioParagraph.className = "post-author-bio";
                bioParagraph.textContent = authorBio;
                bioParagraph.style.fontSize = "0.85em";
                bioParagraph.style.color = "#777";
                bioParagraph.style.marginTop = "-5px";
                bioParagraph.style.marginBottom = "5px";
                bioParagraph.style.whiteSpace = "pre-wrap";
                div.appendChild(bioParagraph);
            }

            const contentParagraph = document.createElement("p");
            contentParagraph.innerHTML = post.content.replace(/\n/g, "<br>");
            div.appendChild(contentParagraph);

            if (post.image) {
                const img = document.createElement("img");
                img.src = post.image;
                img.alt = "Imagem do post";
                div.appendChild(img);
            }

            const postActionsDiv = document.createElement("div");
            postActionsDiv.className = "post-actions";

            if (currentUser === post.author || currentUser === "admin") {
                const editBtn = document.createElement("button");
                editBtn.textContent = translations[currentLanguage].editBtn;
                editBtn.className = "edit-btn-post";
                editBtn.onclick = () => openEditModal(post.id);
                postActionsDiv.appendChild(editBtn);

                const delBtn = document.createElement("button");
                delBtn.textContent = translations[currentLanguage].deleteBtn;
                delBtn.className = "delete-btn";
                delBtn.onclick = () => deletePost(post.id);
                postActionsDiv.appendChild(delBtn);
            }

            if (currentUser) {
                const replyBtn = document.createElement("button");
                replyBtn.textContent = translations[currentLanguage].replyBtn;
                replyBtn.className = "reply-btn";
                replyBtn.onclick = () => toggleReplyBox(post.id);
                postActionsDiv.appendChild(replyBtn);
            }

            div.appendChild(postActionsDiv);

            const replyBox = document.createElement("div");
            replyBox.className = "reply-box";
            replyBox.id = `reply-box-${post.id}`;
            replyBox.innerHTML = `
                <textarea id="reply-text-${post.id}" rows="2" placeholder="${translations[currentLanguage].replyInputPlaceholder}"></textarea>
                <input type="file" id="reply-image-${post.id}" accept="image/*" />
                <button onclick="submitReply(${post.id})">${translations[currentLanguage].sendBtn}</button>
            `;
            div.appendChild(replyBox);


            if (post.replies && post.replies.length > 0) {
                const repliesDiv = document.createElement("div");
                repliesDiv.className = "replies";
                post.replies.sort((a, b) => b.id - a.id);
                post.replies.forEach(reply => {
                    const replyDiv = document.createElement("div");
                    replyDiv.style.borderBottom = "1px solid #eee";
                    replyDiv.style.padding = "4px 0";

                    const replyTextAndActionsContainer = document.createElement("div");
                    replyTextAndActionsContainer.className = "reply-text-and-actions";

                    const replyContentWrapper = document.createElement("div");
                    replyContentWrapper.className = "reply-content-wrapper";

                    const replyAuthorProfilePic = users[reply.author] ? users[reply.author].profilePicture : 'default-profile.png';
                    replyContentWrapper.innerHTML = `
                        <div class="user-info">
                            <img src="${replyAuthorProfilePic}" alt="Foto de Perfil" class="profile-pic-small" onclick="viewUserProfile('${reply.author}')" />
                            <strong onclick="viewUserProfile('${reply.author}')">${reply.author}</strong>: ${reply.content.replace(/\n/g,"<br>")}
                        </div>
                    `;

                    if (reply.image) {
                        replyContentWrapper.innerHTML += `<img src="${reply.image}" alt="Imagem da resposta" />`;
                    }
                    replyTextAndActionsContainer.appendChild(replyContentWrapper);

                    if (currentUser === reply.author || currentUser === "admin") {
                        const replyActionsDiv = document.createElement("div");
                        replyActionsDiv.className = "reply-actions";

                        const editReplyBtn = document.createElement("button");
                        editReplyBtn.textContent = translations[currentLanguage].editBtn;
                        editReplyBtn.onclick = () => openEditReplyModal(post.id, reply.id);
                        replyActionsDiv.appendChild(editReplyBtn);

                        const deleteReplyBtn = document.createElement("button");
                        deleteReplyBtn.textContent = translations[currentLanguage].deleteBtn;
                        deleteReplyBtn.className = "delete-btn";
                        deleteReplyBtn.onclick = () => deleteReply(post.id, reply.id);
                        replyActionsDiv.appendChild(deleteReplyBtn);

                        replyTextAndActionsContainer.appendChild(replyActionsDiv);
                    }
                    replyDiv.appendChild(replyTextAndActionsContainer);
                    repliesDiv.appendChild(replyDiv);
                });
                div.appendChild(repliesDiv);
            }

            return div;
        }

        function toggleReplyBox(postId) {
            const box = document.getElementById(`reply-box-${postId}`);
            if (box.style.display === "block") {
                box.style.display = "none";
                document.getElementById(`reply-text-${postId}`).value = "";
                document.getElementById(`reply-image-${postId}`).value = "";
            } else {
                box.style.display = "block";
            }
        }

        function submitReply(postId) {
            if (!currentUser) return alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].replyBtn.toLowerCase() + ".");
            const textArea = document.getElementById(`reply-text-${postId}`);
            const imageInput = document.getElementById(`reply-image-${postId}`);
            const replyText = textArea.value.trim();

            if (!replyText && !imageInput.files.length) return alert(translations[currentLanguage].alertReplySomething);

            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            let post = posts.find(p => p.id === postId);
            if (!post) return alert(translations[currentLanguage].alertPostNotFound);

            const reader = new FileReader();
            if (imageInput.files.length) {
                reader.onload = function(e) {
                    saveReply(postId, replyText, e.target.result);
                };
                reader.readAsDataURL(imageInput.files[0]);
            } else {
                saveReply(postId, replyText, null);
            }
        }

        function saveReply(postId, replyText, replyImageData) {
            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            let post = posts.find(p => p.id === postId);

            if (!post.replies) post.replies = [];
            post.replies.push({
                id: Date.now(),
                author: currentUser,
                content: replyText,
                image: replyImageData
            });

            localStorage.setItem("tweets", JSON.stringify(posts));
            document.getElementById(`reply-text-${postId}`).value = "";
            document.getElementById(`reply-image-${postId}`).value = "";
            toggleReplyBox(postId);
            loadTweets();
        }

        function deletePost(id) {
            if (!confirm(translations[currentLanguage].alertDeletePostConfirm)) return;
            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            posts = posts.filter(p => p.id !== id);
            localStorage.setItem("tweets", JSON.stringify(posts));
            loadTweets();
        }

        function openEditModal(postId) {
            editPostId = postId;
            editPostImageData = null;
            const posts = JSON.parse(localStorage.getItem("tweets")) || [];
            const post = posts.find(p => p.id === postId);
            if (!post) return alert(translations[currentLanguage].alertPostNotFound);

            document.getElementById("editContent").value = post.content;
            document.getElementById("editImageInput").value = "";

            document.getElementById("editModal").style.display = "flex";
        }

        function closeEditModal() {
            document.getElementById("editModal").style.display = "none";
            editPostId = null;
            editPostImageData = null;
            document.getElementById("editContent").value = "";
            document.getElementById("editImageInput").value = "";
        }

        document.getElementById("editImageInput").addEventListener("change", function(e){
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(event) {
                editPostImageData = event.target.result;
            };
            reader.readAsDataURL(file);
        });

        function saveEdit() {
            if (!editPostId) return;
            const newText = document.getElementById("editContent").value.trim();
            const newImageSelected = document.getElementById("editImageInput").files.length > 0;

            if (!newText && !newImageSelected && editPostImageData === null) {
                return alert(translations[currentLanguage].alertEmptyContent);
            }

            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            const postIndex = posts.findIndex(p => p.id === editPostId);
            if (postIndex < 0) return alert(translations[currentLanguage].alertPostNotFound);

            posts[postIndex].content = newText;
            if (newImageSelected) {
                posts[postIndex].image = editPostImageData;
            } else if (editPostImageData === null && !newImageSelected) {
                posts[postIndex].image = null;
            }

            localStorage.setItem("tweets", JSON.stringify(posts));
            closeEditModal();
            loadTweets();
        }

        function openEditReplyModal(postId, replyId) {
            editReplyData = { postId, replyId, replyImageData: null };
            const posts = JSON.parse(localStorage.getItem("tweets")) || [];
            const post = posts.find(p => p.id === postId);
            if (!post) return alert(translations[currentLanguage].alertPostNotFound);

            const reply = post.replies.find(r => r.id === replyId);
            if (!reply) return alert(translations[currentLanguage].alertReplyNotFound);

            document.getElementById("editReplyContent").value = reply.content;
            document.getElementById("editReplyImageInput").value = "";
            editReplyData.replyImageData = reply.image;

            document.getElementById("editReplyModal").style.display = "flex";
        }

        function closeEditReplyModal() {
            document.getElementById("editReplyModal").style.display = "none";
            editReplyData = { postId: null, replyId: null, replyImageData: null };
            document.getElementById("editReplyContent").value = "";
            document.getElementById("editReplyImageInput").value = "";
        }

        document.getElementById("editReplyImageInput").addEventListener("change", function(e){
            const file = e.target.files[0];
            if (!file) {
                editReplyData.replyImageData = null;
                return;
            }
            const reader = new FileReader();
            reader.onload = function(event) {
                editReplyData.replyImageData = event.target.result;
            };
            reader.readAsDataURL(file);
        });

        function saveReplyEdit() {
            if (!editReplyData.postId || !editReplyData.replyId) return;
            const newReplyText = document.getElementById("editReplyContent").value.trim();
            const newImageSelected = document.getElementById("editReplyImageInput").files.length > 0;

            if (!newReplyText && !newImageSelected && editReplyData.replyImageData === null) {
                return alert(translations[currentLanguage].alertEmptyContent);
            }

            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            const postIndex = posts.findIndex(p => p.id === editReplyData.postId);
            const replyIndex = posts[postIndex].replies.findIndex(r => r.id === editReplyData.replyId);

            posts[postIndex].replies[replyIndex].content = newReplyText;

            if (newImageSelected) {
                posts[postIndex].replies[replyIndex].image = editReplyData.replyImageData;
            } else if (editReplyData.replyImageData === null && !newImageSelected) {
                posts[postIndex].replies[replyIndex].image = null;
            } else if (editReplyData.replyImageData !== null && !newImageSelected) {

            }

            localStorage.setItem("tweets", JSON.stringify(posts));
            closeEditReplyModal();
            loadTweets();
        }

        function deleteReply(postId, replyId) {
            if (!confirm(translations[currentLanguage].alertDeleteReplyConfirm)) return;
            let posts = JSON.parse(localStorage.getItem("tweets")) || [];
            const postIndex = posts.findIndex(p => p.id === postId);
            if (postIndex < 0) return alert(translations[currentLanguage].alertPostNotFound);

            posts[postIndex].replies = posts[postIndex].replies.filter(r => r.id !== replyId);

            localStorage.setItem("tweets", JSON.stringify(posts));
            loadTweets();
        }

        function sendFriendRequest() {
            if (!currentUser) {
                alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].sendFriendRequestButton.toLowerCase() + "!");
                return;
            }

            const targetUsername = document.getElementById("addFriendInput").value.trim();
            if (!targetUsername) {
                alert(translations[currentLanguage].addFriendInput + "!");
                return;
            }
            if (targetUsername === currentUser) {
                alert(translations[currentLanguage].alertSendRequestSelf);
                return;
            }

            let users = getAllUsers();
            const currentUserData = users[currentUser];
            const targetUserData = users[targetUsername];

            if (!targetUserData) {
                alert(`${translations[currentLanguage].alertUserNotExists}.`);
                return;
            }

            if (currentUserData.friends.includes(targetUsername)) {
                alert(`${targetUsername}${translations[currentLanguage].alertAlreadyFriend}`);
                return;
            }
            if (currentUserData.sentRequests.includes(targetUsername)) {
                alert(`${translations[currentLanguage].alertRequestSentAlready}${targetUsername}.`);
                return;
            }
            if (currentUserData.receivedRequests.includes(targetUsername)) {
                alert(`${translations[currentLanguage].alertRequestReceivedAlready}${targetUsername}. ${translations[currentLanguage].acceptBtn}-a na sua lista de solicitações recebidas.`);
                return;
            }

            currentUserData.sentRequests.push(targetUsername);
            targetUserData.receivedRequests.push(currentUser);

            localStorage.setItem("users", JSON.stringify(users));
            alert(`${translations[currentLanguage].alertFriendRequestSent}${targetUsername}!`);
            document.getElementById("addFriendInput").value = "";
            loadFriendStatus();
            closeUserProfileView(); // Re-render profile view if active
        }

        function acceptFriendRequest(senderUsername) {
            if (!currentUser) return;

            let users = getAllUsers();
            const currentUserData = users[currentUser];
            const senderUserData = users[senderUsername];

            if (!currentUserData || !senderUserData) {
                alert("Erro: Usuário ou solicitante não encontrado.");
                return;
            }

            currentUserData.receivedRequests = currentUserData.receivedRequests.filter(req => req !== senderUsername);
            senderUserData.sentRequests = senderUserData.sentRequests.filter(req => req !== currentUser);

            if (!currentUserData.friends.includes(senderUsername)) {
                currentUserData.friends.push(senderUsername);
            }
            if (!senderUserData.friends.includes(currentUser)) {
                senderUserData.friends.push(currentUser);
            }

            localStorage.setItem("users", JSON.stringify(users));
            alert(`${translations[currentLanguage].alertAcceptFriendRequest}${senderUsername}${translations[currentLanguage].alertNowFriends}`);
            loadFriendStatus();
            loadChatFriendsList();
            loadGroupMembersForCreation();
            closeUserProfileView(); // Re-render profile view if active
            loadStatuses(); // NEW: Reload statuses when friends list changes
        }

        function rejectFriendRequest(senderUsername) {
            if (!currentUser) return;
            if (!confirm(`${translations[currentLanguage].alertRejectFriendRequestConfirm}${senderUsername}?`)) return;

            let users = getAllUsers();
            const currentUserData = users[currentUser];
            const senderUserData = users[senderUsername];

            if (!currentUserData || !senderUserData) {
                alert("Erro: Usuário ou solicitante não encontrado.");
                return;
            }

            currentUserData.receivedRequests = currentUserData.receivedRequests.filter(req => req !== senderUsername);
            senderUserData.sentRequests = senderUserData.sentRequests.filter(req => req !== currentUser);

            localStorage.setItem("users", JSON.stringify(users));
            alert(`${translations[currentLanguage].alertRejectedFriendRequest}${senderUsername}.`);
            loadFriendStatus();
            closeUserProfileView(); // Re-render profile view if active
        }

        function removeFriend(friendUsername) {
            if (!currentUser) return;
            if (!confirm(`${translations[currentLanguage].alertRemoveFriendConfirm}${friendUsername}${translations[currentLanguage].removeFriendBtn.toLowerCase()}?`)) return;

            let users = getAllUsers();
            const currentUserData = users[currentUser];
            const friendUserData = users[friendUsername];

            if (!currentUserData || !friendUserData) {
                alert("Erro: Usuário ou amigo não encontrado.");
                return;
            }

            currentUserData.friends = currentUserData.friends.filter(friend => friend !== friendUsername);
            friendUserData.friends = friendUserData.friends.filter(friend => friend !== currentUser);

            let messages = getAllMessages();
            const conversationId = getConversationId(currentUser, friendUsername);
            delete messages[conversationId];
            localStorage.setItem("chatMessages", JSON.stringify(messages));


            localStorage.setItem("users", JSON.stringify(users));
            alert(`${friendUsername}${translations[currentLanguage].alertFriendRemoved}`);
            loadFriendStatus();
            loadChatFriendsList();
            loadGroupSection();
            if (currentChatFriend === friendUsername) {
                closeChat();
            }
            closeUserProfileView(); // Re-render profile view if active
            loadStatuses(); // NEW: Reload statuses when friends list changes
        }

        function loadFriendStatus() {
            const friendsList = document.getElementById("friendsList");
            const sentRequestsList = document.getElementById("sentRequestsList");
            const receivedRequestsList = document.getElementById("receivedRequestsList");

            friendsList.innerHTML = "";
            sentRequestsList.innerHTML = "";
            receivedRequestsList.innerHTML = "";

            if (currentUser) {
                const users = getAllUsers();
                const currentUserData = users[currentUser];

                if (currentUserData) {
                    if (Array.isArray(currentUserData.friends)) {
                        currentUserData.friends.forEach(friend => {
                            const li = document.createElement("li");
                            li.textContent = friend;
                            const removeBtn = document.createElement("button");
                            removeBtn.textContent = translations[currentLanguage].removeFriendBtn;
                            removeBtn.onclick = () => removeFriend(friend);
                            li.appendChild(removeBtn);
                            friendsList.appendChild(li);
                        });
                    }

                    if (Array.isArray(currentUserData.sentRequests)) {
                        currentUserData.sentRequests.forEach(req => {
                            const li = document.createElement("li");
                            li.textContent = req;
                            sentRequestsList.appendChild(li);
                        });
                    }

                    if (Array.isArray(currentUserData.receivedRequests)) {
                        currentUserData.receivedRequests.forEach(req => {
                            const li = document.createElement("li");
                            li.textContent = req;
                            const acceptBtn = document.createElement("button");
                            acceptBtn.textContent = translations[currentLanguage].acceptBtn;
                            acceptBtn.className = "accept-btn";
                            acceptBtn.onclick = () => acceptFriendRequest(req);
                            const rejectBtn = document.createElement("button");
                            rejectBtn.textContent = translations[currentLanguage].rejectBtn;
                            rejectBtn.className = "reject-btn";
                            rejectBtn.onclick = () => rejectFriendRequest(req);
                            li.appendChild(acceptBtn);
                            li.appendChild(rejectBtn);
                            receivedRequestsList.appendChild(li);
                        });
                    }
                }
            }
        }

        function loadChatFriendsList() {
            const chatFriendsList = document.getElementById("chatFriendsList");
            chatFriendsList.innerHTML = "";

            if (!currentUser) {
                document.getElementById("chatSection").style.display = "none";
                return;
            }
            document.getElementById("chatSection").style.display = "block";

            const users = getAllUsers();
            const currentUserData = users[currentUser];

            if (currentUserData && Array.isArray(currentUserData.friends)) {
                if (currentUserData.friends.length === 0) {
                    const li = document.createElement("li");
                    li.textContent = translations[currentLanguage].noFriendsChat;
                    li.style.fontStyle = "italic";
                    chatFriendsList.appendChild(li);
                } else {
                    currentUserData.friends.forEach(friend => {
                        const li = document.createElement("li");
                        li.textContent = friend;
                        li.setAttribute("data-friend", friend);
                        li.onclick = () => startChat(friend);
                        if (friend === currentChatFriend) {
                            li.classList.add("selected");
                        }
                        chatFriendsList.appendChild(li);
                    });
                }
            }
        }

        function getConversationId(user1, user2) {
            return [user1, user2].sort().join('-');
        }

        function startChat(friendUsername) {
            if (!currentUser) return alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].chatSectionTitle.toLowerCase() + ".");

            closeGroupChat();
            closeUserProfileView(); // Close profile view when starting a chat

            currentChatFriend = friendUsername;

            const chatFriendsListItems = document.querySelectorAll("#chatFriendsList li");
            chatFriendsListItems.forEach(item => {
                if (item.getAttribute("data-friend") === friendUsername) {
                    item.classList.add("selected");
                } else {
                    item.classList.remove("selected");
                }
            });

            document.getElementById("chatBox").style.display = "block";
            document.getElementById("chatFriendName").textContent = `${translations[currentLanguage].chatHeader} ${friendUsername}`;
            loadChatMessages(friendUsername);
            document.getElementById("chatMessageInput").focus();
        }

        function closeChat() {
            document.getElementById("chatBox").style.display = "none";
            currentChatFriend = null;
            const chatFriendsListItems = document.querySelectorAll("#chatFriendsList li");
            chatFriendsListItems.forEach(item => {
                item.classList.remove("selected");
            });
            document.getElementById("chatMessageInput").value = "";
            document.getElementById("chatImageInput").value = "";
        }


        function loadChatMessages(friendUsername) {
            const messagesDisplay = document.getElementById("messagesDisplay");
            messagesDisplay.innerHTML = "";
            const messages = getAllMessages();
            const users = getAllUsers();
            const conversationId = getConversationId(currentUser, friendUsername);

            const chatHistory = messages[conversationId] || [];

            if (chatHistory.length === 0) {
                messagesDisplay.innerHTML = `<p style='text-align: center; color: #888;'>${translations[currentLanguage].noMessagesYet}</p>`;
            } else {
                chatHistory.forEach(msg => {
                    const msgDiv = document.createElement("div");
                    msgDiv.classList.add("message");

                    const messageContent = document.createElement("div");
                    messageContent.classList.add("message-content");

                    const senderProfilePic = users[msg.sender] ? users[msg.sender].profilePicture : 'default-profile.png';

                    const userInfoDiv = document.createElement("div");
                    userInfoDiv.className = "user-info";
                    userInfoDiv.innerHTML = `
                        <img src="${senderProfilePic}" alt="Foto de Perfil" class="profile-pic-small" onclick="viewUserProfile('${msg.sender}')" />
                        <strong onclick="viewUserProfile('${msg.sender}')">${msg.sender === currentUser ? translations[currentLanguage].you : msg.sender}</strong>
                    `;
                    messageContent.appendChild(userInfoDiv);

                    const messageTextP = document.createElement("p");
                    messageTextP.textContent = msg.text;
                    messageContent.appendChild(messageTextP);

                    if (msg.image) {
                        const img = document.createElement("img");
                        img.src = msg.image;
                        img.alt = "Imagem da mensagem";
                        messageContent.appendChild(img);
                    }
                    msgDiv.appendChild(messageContent);

                    if (currentUser === msg.sender || currentUser === "admin") {
                        const optionsDiv = document.createElement("div");
                        optionsDiv.classList.add("message-options");

                        const editBtn = document.createElement("button");
                        editBtn.textContent = translations[currentLanguage].editBtn;
                        editBtn.classList.add("edit-btn");
                        editBtn.onclick = () => openEditChatMessageModal('individual', conversationId, msg.id);
                        optionsDiv.appendChild(editBtn);

                        const deleteBtn = document.createElement("button");
                        deleteBtn.textContent = translations[currentLanguage].deleteBtn;
                        deleteBtn.classList.add("delete-btn");
                        deleteBtn.onclick = () => deleteChatMessage('individual', conversationId, msg.id);
                        optionsDiv.appendChild(deleteBtn);

                        msgDiv.appendChild(optionsDiv);
                    }


                    messagesDisplay.appendChild(msgDiv);
                });
            }
            messagesDisplay.scrollTop = messagesDisplay.scrollHeight;
        }

        function sendChatMessage() {
            if (!currentUser || !currentChatFriend) {
                alert(translations[currentLanguage].alertSelectFriendChat);
                return;
            }
            const messageInput = document.getElementById("chatMessageInput");
            const imageInput = document.getElementById("chatImageInput");
            const messageText = messageInput.value.trim();
            const imageFile = imageInput.files[0];

            if (!messageText && !imageFile) {
                return;
            }

            const saveChat = (imageData) => {
                let messages = getAllMessages();
                const conversationId = getConversationId(currentUser, currentChatFriend);

                if (!messages[conversationId]) {
                    messages[conversationId] = [];
                }

                messages[conversationId].push({
                    id: Date.now(),
                    sender: currentUser,
                    text: messageText,
                    image: imageData,
                    timestamp: Date.now()
                });

                localStorage.setItem("chatMessages", JSON.stringify(messages));
                messageInput.value = "";
                imageInput.value = "";
                loadChatMessages(currentChatFriend);
            };

            if (imageFile) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    saveChat(e.target.result);
                };
                reader.readAsDataURL(imageFile);
            } else {
                saveChat(null);
            }
        }

        function openEditChatMessageModal(chatType, conversationId, messageId) {
            editChatData = { chatType, conversationId, messageId, messageImageData: null };

            let messages;
            let currentMessage;

            if (chatType === 'individual') {
                messages = getAllMessages();
                currentMessage = messages[conversationId].find(msg => msg.id === messageId);
            } else {
                const groups = getAllGroups();
                const group = groups.find(g => g.id == conversationId);
                currentMessage = group.messages.find(msg => msg.id === messageId);
            }

            if (!currentMessage) {
                alert(translations[currentLanguage].alertMessageNotFound);
                return;
            }

            document.getElementById("editChatMessageContent").value = currentMessage.text;
            document.getElementById("editChatMessageImageInput").value = "";
            editChatData.messageImageData = currentMessage.image;

            document.getElementById("editChatMessageModal").style.display = "flex";
            document.getElementById("editChatMessageContent").focus();
        }

        function closeEditChatMessageModal() {
            document.getElementById("editChatMessageModal").style.display = "none";
            editChatData = { chatType: null, conversationId: null, messageId: null, messageImageData: null };
            document.getElementById("editChatMessageContent").value = "";
            document.getElementById("editChatMessageImageInput").value = "";
        }

        document.getElementById("editChatMessageImageInput").addEventListener("change", function(e){
            const file = e.target.files[0];
            if (!file) {
                editChatData.messageImageData = null;
                return;
            }
            const reader = new FileReader();
            reader.onload = function(event) {
                editChatData.messageImageData = event.target.result;
            };
            reader.readAsDataURL(file);
        });

        function saveChatMessageEdit() {
            if (!editChatData.chatType || !editChatData.conversationId || !editChatData.messageId) return;

            const newText = document.getElementById("editChatMessageContent").value.trim();
            const newImageSelected = document.getElementById("editChatMessageImageInput").files.length > 0;

            if (!newText && !newImageSelected && editChatData.messageImageData === null) {
                return alert(translations[currentLanguage].alertEmptyContent);
            }

            if (editChatData.chatType === 'individual') {
                let messages = getAllMessages();
                const conversationHistory = messages[editChatData.conversationId];
                if (!conversationHistory) return;

                const messageIndex = conversationHistory.findIndex(msg => msg.id === editChatData.messageId);
                if (messageIndex === -1) return;

                conversationHistory[messageIndex].text = newText;
                if (newImageSelected) {
                    conversationHistory[messageIndex].image = editChatData.messageImageData;
                } else if (editChatData.messageImageData === null && !newImageSelected) {
                    conversationHistory[messageIndex].image = null;
                }

                localStorage.setItem("chatMessages", JSON.stringify(messages));
                loadChatMessages(currentChatFriend);

            } else {
                let groups = getAllGroups();
                const groupIndex = groups.findIndex(g => g.id == editChatData.conversationId);
                if (groupIndex === -1) return;

                const groupMessages = groups[groupIndex].messages;
                const messageIndex = groupMessages.findIndex(msg => msg.id === editChatData.messageId);
                if (messageIndex === -1) return;

                groupMessages[messageIndex].text = newText;
                if (newImageSelected) {
                    groupMessages[messageIndex].image = editChatData.messageImageData;
                } else if (editChatData.messageImageData === null && !newImageSelected) {
                    groupMessages[messageIndex].image = null;
                }

                localStorage.setItem("groups", JSON.stringify(groups));
                loadGroupMessages(currentGroupChatId);
            }
            closeEditChatMessageModal();
        }

        function deleteChatMessage(chatType, conversationId, messageId) {
            if (!confirm(translations[currentLanguage].alertDeleteMessageConfirm)) return;

            if (chatType === 'individual') {
                let messages = getAllMessages();
                if (messages[conversationId]) {
                    messages[conversationId] = messages[conversationId].filter(msg => msg.id !== messageId);
                    localStorage.setItem("chatMessages", JSON.stringify(messages));
                    loadChatMessages(currentChatFriend);
                }
            } else {
                let groups = getAllGroups();
                const groupIndex = groups.findIndex(g => g.id !==-1);
                if (groupIndex !== -1) {
                    groups[groupIndex].messages = groups[groupIndex].messages.filter(msg => msg.id !== messageId);
                    localStorage.setItem("groups", JSON.stringify(groups));
                    loadGroupMessages(currentGroupChatId);
                }
            }
        }


        document.getElementById("chatMessageInput").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                event.preventDefault();
                sendChatMessage();
            }
        });

        function loadGroupSection() {
            const groupSection = document.getElementById("groupSection");
            if (!currentUser) {
                groupSection.style.display = "none";
                return;
            }
            groupSection.style.display = "block";
            loadGroupMembersForCreation();
            loadMyGroups();
        }

        function loadGroupMembersForCreation() {
            const groupMembersList = document.getElementById("groupMembersList");
            groupMembersList.innerHTML = "";

            const users = getAllUsers();
            const currentUserData = users[currentUser];

            if (currentUserData && Array.isArray(currentUserData.friends)) {
                if (currentUserData.friends.length === 0) {
                    const li = document.createElement("li");
                    li.textContent = translations[currentLanguage].noFriendsChat.replace("para conversar", "para criar grupos");
                    li.style.fontStyle = "italic";
                    groupMembersList.appendChild(li);
                } else {
                    currentUserData.friends.forEach(friend => {
                        const li = document.createElement("li");
                        const checkbox = document.createElement("input");
                        checkbox.type = "checkbox";
                        checkbox.id = `group-member-${friend}`;
                        checkbox.value = friend;

                        const label = document.createElement("label");
                        label.htmlFor = `group-member-${friend}`;
                        label.textContent = friend;

                        li.appendChild(checkbox);
                        li.appendChild(label);
                        groupMembersList.appendChild(li);
                    });
                }
            }
        }

        function createGroup() {
            if (!currentUser) {
                alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].createGroupBtn.toLowerCase() + "!");
                return;
            }

            const groupName = document.getElementById("groupNameInput").value.trim();
            if (!groupName) {
                alert(translations[currentLanguage].alertGroupNameRequired);
                return;
            }

            let groups = getAllGroups();
            if (groups.some(group => group.name.toLowerCase() === groupName.toLowerCase())) {
                alert(translations[currentLanguage].alertGroupExists);
                return;
            }

            const selectedMembers = [currentUser];
            const checkboxes = document.querySelectorAll('#groupMembersList input[type="checkbox"]:checked');
            checkboxes.forEach(checkbox => {
                selectedMembers.push(checkbox.value);
            });

            if (selectedMembers.length < 2) {
                alert(translations[currentLanguage].alertGroupMinMembers);
                return;
            }

            const newGroup = {
                id: Date.now(),
                name: groupName,
                creator: currentUser,
                members: selectedMembers,
                messages: []
            };

            groups.push(newGroup);
            localStorage.setItem("groups", JSON.stringify(groups));

            alert(`Grupo "${groupName}" ${translations[currentLanguage].alertGroupCreated}`);
            document.getElementById("groupNameInput").value = "";
            checkboxes.forEach(checkbox => checkbox.checked = false);
            loadMyGroups();
        }

        function loadMyGroups() {
            const myGroupsList = document.getElementById("myGroupsList");
            myGroupsList.innerHTML = "";
            const groups = getAllGroups();

            if (!currentUser) return;

            const userGroups = groups.filter(group => group.members.includes(currentUser));

            if (userGroups.length === 0) {
                const li = document.createElement("li");
                li.textContent = translations[currentLanguage].noGroupsYet;
                li.style.fontStyle = "italic";
                myGroupsList.appendChild(li);
            } else {
                userGroups.forEach(group => {
                    const li = document.createElement("li");
                    li.textContent = group.name;
                    li.setAttribute("data-group-id", group.id);
                    li.onclick = () => startGroupChat(group.id);
                    if (group.id === currentGroupChatId) {
                        li.classList.add("selected");
                    }
                    myGroupsList.appendChild(li);
                });
            }
        }

        function startGroupChat(groupId) {
            if (!currentUser) return alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].groupChatHeader.toLowerCase() + ".");

            closeChat();
            closeUserProfileView(); // Close profile view when starting group chat

            currentGroupChatId = groupId;

            const myGroupsListItems = document.querySelectorAll("#myGroupsList li");
            myGroupsListItems.forEach(item => {
                if (item.getAttribute("data-group-id") == groupId) {
                    item.classList.add("selected");
                } else {
                    item.classList.remove("selected");
                }
            });

            const groups = getAllGroups();
            const selectedGroup = groups.find(g => g.id === groupId);

            if (!selectedGroup) {
                alert(translations[currentLanguage].alertGroupNotFound);
                return;
            }

            document.getElementById("groupChatBox").style.display = "block";
            document.getElementById("groupChatName").textContent = `${translations[currentLanguage].groupChatHeader} ${selectedGroup.name}`;
            loadGroupMessages(groupId);
            document.getElementById("groupChatMessageInput").focus();
        }

        function closeGroupChat() {
            document.getElementById("groupChatBox").style.display = "none";
            currentGroupChatId = null;
            const myGroupsListItems = document.querySelectorAll("#myGroupsList li");
            myGroupsListItems.forEach(item => {
                item.classList.remove("selected");
            });
            document.getElementById("groupChatMessageInput").value = "";
            document.getElementById("groupChatImageInput").value = "";
        }

        function loadGroupMessages(groupId) {
            const messagesDisplay = document.getElementById("groupMessagesDisplay");
            messagesDisplay.innerHTML = "";
            const groups = getAllGroups();
            const users = getAllUsers();
            const group = groups.find(g => g.id === groupId);

            if (!group || !group.messages) {
                messagesDisplay.innerHTML = `<p style='text-align: center; color: #888;'>${translations[currentLanguage].noMessagesYet}</p>`;
                return;
            }

            if (group.messages.length === 0) {
                messagesDisplay.innerHTML = `<p style='text-align: center; color: #888;'>${translations[currentLanguage].noMessagesYet}</p>`;
            } else {
                group.messages.forEach(msg => {
                    const msgDiv = document.createElement("div");
                    msgDiv.classList.add("message");

                    const messageContent = document.createElement("div");
                    messageContent.classList.add("message-content");

                    const senderProfilePic = users[msg.sender] ? users[msg.sender].profilePicture : 'default-profile.png';

                    const userInfoDiv = document.createElement("div");
                    userInfoDiv.className = "user-info";
                    userInfoDiv.innerHTML = `
                        <img src="${senderProfilePic}" alt="Foto de Perfil" class="profile-pic-small" onclick="viewUserProfile('${msg.sender}')" />
                        <strong onclick="viewUserProfile('${msg.sender}')">${msg.sender === currentUser ? translations[currentLanguage].you : msg.sender}</strong>
                    `;
                    messageContent.appendChild(userInfoDiv);

                    const messageTextP = document.createElement("p");
                    messageTextP.textContent = msg.text;
                    messageContent.appendChild(messageTextP);

                    if (msg.image) {
                        const img = document.createElement("img");
                        img.src = msg.image;
                        img.alt = "Imagem da mensagem";
                        messageContent.appendChild(img);
                    }
                    msgDiv.appendChild(messageContent);

                    if (currentUser === msg.sender || currentUser === "admin") {
                        const optionsDiv = document.createElement("div");
                        optionsDiv.classList.add("message-options");

                        const editBtn = document.createElement("button");
                        editBtn.textContent = translations[currentLanguage].editBtn;
                        editBtn.classList.add("edit-btn");
                        editBtn.onclick = () => openEditChatMessageModal('group', groupId, msg.id);
                        optionsDiv.appendChild(editBtn);

                        const deleteBtn = document.createElement("button");
                        deleteBtn.textContent = translations[currentLanguage].deleteBtn;
                        deleteBtn.classList.add("delete-btn");
                        deleteBtn.onclick = () => deleteChatMessage('group', groupId, msg.id);
                        optionsDiv.appendChild(deleteBtn);

                        msgDiv.appendChild(optionsDiv);
                    }

                    messagesDisplay.appendChild(msgDiv);
                });
            }
            messagesDisplay.scrollTop = messagesDisplay.scrollHeight;
        }

        // User Profile View Functions
        function viewUserProfile(usernameToView) {
            const userProfileView = document.getElementById("userProfileView");
            const profileViewUsername = document.getElementById("profileViewUsername");
            const profileViewPic = document.getElementById("profileViewPic");
            const profileViewName = document.getElementById("profileViewName");
            const profileViewBio = document.getElementById("profileViewBio");
            const profileViewActions = document.getElementById("profileViewActions");

            const users = getAllUsers();
            const userToView = users[usernameToView];

            if (!userToView) {
                alert(translations[currentLanguage].alertUserNotExists);
                return;
            }

            // Hide other sections if open
            closeChat();
            closeGroupChat();
            // hide user management stuff as well
            document.getElementById("usernameInput").style.display = 'none';
            document.getElementById("passwordInput").style.display = 'none';
            document.getElementById("bioInput").style.display = 'none';
            document.getElementById("editUserBtn").style.display = 'none';
            document.getElementById("editBioBtn").style.display = 'none';
            document.getElementById("deleteAccountBtn").style.display = 'none';
            document.getElementById("registerBtn").style.display = 'none';
            document.getElementById("loginBtn").style.display = 'none';
            document.getElementById("logoutBtn").style.display = 'none';
            document.getElementById("adminPanel").style.display = 'none';
            document.getElementById("friendsSection").style.display = 'none';
            document.getElementById("chatSection").style.display = 'none';
            document.getElementById("groupSection").style.display = 'none';
            document.getElementById("profilePictureInput").style.display = 'none';
            document.getElementById("uploadProfilePictureBtn").style.display = 'none';
            document.getElementById("removeProfilePictureBtn").style.display = 'none';
            // NEW: Hide status section when viewing profile
            document.getElementById("statusSection").style.display = 'none';


            userProfileView.style.display = "block";
            profileViewUsername.textContent = `${translations[currentLanguage].profileViewTitle}${usernameToView}`;
            profileViewName.textContent = usernameToView;
            profileViewPic.src = userToView.profilePicture || 'default-profile.png';
            profileViewBio.textContent = userToView.bio || translations[currentLanguage].bioPlaceholder;

            profileViewActions.innerHTML = ""; // Clear existing buttons

            if (currentUser && currentUser !== usernameToView) {
                const currentUserData = users[currentUser];
                if (currentUserData.friends.includes(usernameToView)) {
                    // Already friends
                    const chatBtn = document.createElement("button");
                    chatBtn.textContent = translations[currentLanguage].profileViewChat;
                    chatBtn.className = "chat-btn";
                    chatBtn.onclick = () => startChat(usernameToView);

                    const removeFriendBtn = document.createElement("button");
                    removeFriendBtn.textContent = translations[currentLanguage].profileViewRemoveFriend;
                    removeFriendBtn.className = "remove-friend-btn";
                    removeFriendBtn.onclick = () => removeFriend(usernameToView);

                    profileViewActions.appendChild(chatBtn);
                    profileViewActions.appendChild(removeFriendBtn);

                } else if (currentUserData.sentRequests.includes(usernameToView)) {
                    // Sent request, pending
                    const pendingBtn = document.createElement("button");
                    pendingBtn.textContent = translations[currentLanguage].profileViewPendingRequest;
                    pendingBtn.className = "pending-request-btn";
                    pendingBtn.disabled = true; // Cannot click
                    profileViewActions.appendChild(pendingBtn);
                } else if (currentUserData.receivedRequests.includes(usernameToView)) {
                    // Received request, can accept or reject
                    const acceptBtn = document.createElement("button");
                    acceptBtn.textContent = translations[currentLanguage].profileViewAcceptRequest;
                    acceptBtn.className = "received-request-accept-btn";
                    acceptBtn.onclick = () => acceptFriendRequest(usernameToView);

                    const rejectBtn = document.createElement("button");
                    rejectBtn.textContent = translations[currentLanguage].profileViewRejectRequest;
                    rejectBtn.className = "received-request-reject-btn";
                    rejectBtn.onclick = () => rejectFriendRequest(usernameToView);

                    profileViewActions.appendChild(acceptBtn);
                    profileViewActions.appendChild(rejectBtn);
                } else {
                    // Not friends, can send request
                    const sendRequestBtn = document.createElement("button");
                    sendRequestBtn.textContent = translations[currentLanguage].profileViewSendRequest;
                    sendRequestBtn.onclick = () => {
                        document.getElementById("addFriendInput").value = usernameToView;
                        sendFriendRequest();
                    };
                    profileViewActions.appendChild(sendRequestBtn);
                }
            } else if (currentUser && currentUser === usernameToView) {
                // Viewing own profile, show edit buttons (optional, as main sidebar has them)
                const editProfileBtn = document.createElement("button");
                editProfileBtn.textContent = translations[currentLanguage].editUserBtn;
                editProfileBtn.onclick = () => {
                    closeUserProfileView(); // Close view to reveal main edit options
                    document.getElementById("usernameInput").focus();
                };
                // profileViewActions.appendChild(editProfileBtn); // Maybe not needed if main controls are sufficient
            }
        }

        function closeUserProfileView() {
            document.getElementById("userProfileView").style.display = "none";
            // Restore visibility of main sidebar controls based on login status
            if (currentUser) {
                document.getElementById("usernameInput").style.display = 'block';
                document.getElementById("passwordInput").style.display = 'block';
                document.getElementById("bioInput").style.display = 'block';
                document.getElementById("editUserBtn").style.display = 'block';
                document.getElementById("editBioBtn").style.display = 'block';
                document.getElementById("deleteAccountBtn").style.display = 'block';
                document.getElementById("uploadProfilePictureBtn").style.display = 'block';
                document.getElementById("removeProfilePictureBtn").style.display = 'block';
                document.getElementById("friendsSection").style.display = 'block';
                document.getElementById("chatSection").style.display = 'block';
                document.getElementById("groupSection").style.display = 'block';
                document.getElementById("statusSection").style.display = 'block'; // NEW: Restore status section
                showAdminPanel(); // Re-show admin panel if admin is logged in
            } else {
                document.getElementById("usernameInput").style.display = 'block';
                document.getElementById("passwordInput").style.display = 'block';
                document.getElementById("registerBtn").style.display = 'block';
                document.getElementById("loginBtn").style.display = 'block';
            }
            document.getElementById("logoutBtn").style.display = currentUser ? 'block' : 'none';
        }

        // NEW: Status Functions

        function postStatus() {
            if (!currentUser) {
                alert(translations[currentLanguage].alertLoginRequired + translations[currentLanguage].postStatusBtn.toLowerCase() + "!");
                return;
            }
            const text = document.getElementById("statusTextInput").value.trim();
            const imageInput = document.getElementById("statusImageInput");
            if (!text && !imageInput.files.length) return alert(translations[currentLanguage].alertStatusSomething);

            const reader = new FileReader();
            if (imageInput.files.length) {
                reader.onload = function(e) {
                    saveStatus(text, e.target.result);
                };
                reader.readAsDataURL(imageInput.files[0]);
            } else {
                saveStatus(text, null);
            }
        }

        function saveStatus(text, imageData) {
            let users = getAllUsers();
            if (users[currentUser]) {
                users[currentUser].status = {
                    id: Date.now(),
                    content: text,
                    image: imageData,
                    timestamp: Date.now()
                };
                localStorage.setItem("users", JSON.stringify(users));
                document.getElementById("statusTextInput").value = "";
                document.getElementById("statusImageInput").value = "";
                loadStatuses();
            }
        }

        function loadStatuses() {
            const myStatusDisplay = document.getElementById("myStatusDisplay");
            const friendStatusesList = document.getElementById("friendStatusesList");
            myStatusDisplay.innerHTML = "";
            friendStatusesList.innerHTML = "";

            if (!currentUser) {
                myStatusDisplay.style.display = "none";
                return;
            }

            const users = getAllUsers();
            const currentUserData = users[currentUser];

            // Display My Status
            if (currentUserData && currentUserData.status) {
                myStatusDisplay.style.display = "block";
                myStatusDisplay.innerHTML = `
                    <div class="status-content">${currentUserData.status.content.replace(/\n/g, "<br>")}</div>
                    ${currentUserData.status.image ? `<img src="${currentUserData.status.image}" alt="Status Image" />` : ''}
                    <div class="status-time">${new Date(currentUserData.status.timestamp).toLocaleString(currentLanguage)}</div>
                    <div class="status-actions">
                        <button class="edit-btn" onclick="openEditStatusModal('${currentUserData.status.id}')">${translations[currentLanguage].editBtn}</button>
                        <button class="delete-btn" onclick="deleteStatus('${currentUserData.status.id}')">${translations[currentLanguage].deleteBtn}</button>
                    </div>
                `;
            } else {
                myStatusDisplay.style.display = "none";
                const p = document.createElement("p");
                p.textContent = translations[currentLanguage].noStatusYet;
                p.style.fontStyle = "italic";
                myStatusDisplay.appendChild(p);
            }

            // Display Friend Statuses
            const friendStatuses = [];
            if (currentUserData && Array.isArray(currentUserData.friends)) {
                currentUserData.friends.forEach(friendUsername => {
                    const friendData = users[friendUsername];
                    if (friendData && friendData.status) {
                        friendStatuses.push({
                            username: friendUsername,
                            status: friendData.status,
                            profilePic: friendData.profilePicture
                        });
                    }
                });
            }

            if (friendStatuses.length === 0) {
                const li = document.createElement("li");
                li.textContent = translations[currentLanguage].noFriendStatuses;
                li.style.fontStyle = "italic";
                friendStatusesList.appendChild(li);
            } else {
                friendStatuses.sort((a, b) => b.status.timestamp - a.status.timestamp); // Sort by most recent
                friendStatuses.forEach(data => {
                    const li = document.createElement("li");
                    li.innerHTML = `
                        <div class="user-info">
                            <img src="${data.profilePic || 'default-profile.png'}" alt="Foto de Perfil" class="profile-pic-small" onclick="viewUserProfile('${data.username}')" />
                            <strong onclick="viewUserProfile('${data.username}')">${data.username}</strong>
                        </div>
                        <div class="status-content">${data.status.content.replace(/\n/g, "<br>")}</div>
                        ${data.status.image ? `<img src="${data.status.image}" alt="Status Image" />` : ''}
                        <div class="status-time">${new Date(data.status.timestamp).toLocaleString(currentLanguage)}</div>
                    `;
                    friendStatusesList.appendChild(li);
                });
            }
        }

        function openEditStatusModal(statusId) {
            editStatusData = { statusId, statusImageData: null };
            let users = getAllUsers();
            const user = users[currentUser];

            if (!user || !user.status || user.status.id != statusId) {
                alert(translations[currentLanguage].alertStatusNotFound);
                return;
            }

            document.getElementById("editStatusContent").value = user.status.content;
            document.getElementById("editStatusImageInput").value = "";
            editStatusData.statusImageData = user.status.image;

            document.getElementById("editStatusModal").style.display = "flex";
            document.getElementById("editStatusContent").focus();
        }

        function closeEditStatusModal() {
            document.getElementById("editStatusModal").style.display = "none";
            editStatusData = { statusId: null, statusImageData: null };
            document.getElementById("editStatusContent").value = "";
            document.getElementById("editStatusImageInput").value = "";
        }

        document.getElementById("editStatusImageInput").addEventListener("change", function(e) {
            const file = e.target.files[0];
            if (!file) {
                editStatusData.statusImageData = null;
                return;
            }
            const reader = new FileReader();
            reader.onload = function(event) {
                editStatusData.statusImageData = event.target.result;
            };
            reader.readAsDataURL(file);
        });

        function saveStatusEdit() {
            if (!editStatusData.statusId) return;

            const newText = document.getElementById("editStatusContent").value.trim();
            const newImageSelected = document.getElementById("editStatusImageInput").files.length > 0;

            if (!newText && !newImageSelected && editStatusData.statusImageData === null) {
                return alert(translations[currentLanguage].alertEmptyContent);
            }

            let users = getAllUsers();
            const user = users[currentUser];

            if (!user || !user.status || user.status.id != editStatusData.statusId) {
                alert(translations[currentLanguage].alertStatusNotFound);
                return;
            }

            user.status.content = newText;
            if (newImageSelected) {
                user.status.image = editStatusData.statusImageData;
            } else if (editStatusData.statusImageData === null && !newImageSelected) {
                user.status.image = null;
            }
            user.status.timestamp = Date.now(); // Update timestamp on edit

            localStorage.setItem("users", JSON.stringify(users));
            alert(translations[currentLanguage].alertStatusUpdated);
            closeEditStatusModal();
            loadStatuses();
        }

        function deleteStatus(statusId) {
            if (!currentUser) return;
            if (!confirm(translations[currentLanguage].alertDeleteStatusConfirm)) return;

            let users = getAllUsers();
            const user = users[currentUser];

            if (user && user.status && user.status.id == statusId) {
                user.status = null;
                localStorage.setItem("users", JSON.stringify(users));
                alert(translations[currentLanguage].alertStatusRemoved);
                loadStatuses();
            } else {
                alert(translations[currentLanguage].alertStatusNotFound);
            }
        }


        applyMode();
        setLanguage(currentLanguage);
    </script>

</body>
</html>
