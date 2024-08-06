






global function RTKDeathScreenSquadSummary_PopulateData



struct
{
	rtk_struct squadSummary = null
} file

global struct RTKDeathScreenSquadSummary_Properties
{
	rtk_panel summaryCardList
}

global struct RTKSquadSummaryCardStatModel
{
	string title
	string value
}

global struct RTKSquadSummaryCardModel
{
	string playerName = ""
	string hardware
	string platformIDString
	array<RTKSquadSummaryCardStatModel> stats
	string obfuscatedID
	string uid
	string unspoofedUID
	string playerEAID
	string actionText = ""
	bool isLocalPlayer

	bool canMute = false
	bool isMuted = false
	bool canViewProfile = false

	bool canReport = false
	bool canInvite = false
	bool canSendFriendRequest = false
	bool isConnected = true
}

global struct RTKSquadSummaryModel
{
	array<RTKSquadSummaryCardModel> cards
}



































































































































































































































































void function RTKDeathScreenSquadSummary_PopulateData( string path = "&menus.squadSummary")
{
	if ( !RTKDataModel_HasDataModel( path ) )
		return

	rtk_struct model = RTKDataModel_GetStruct( path )
	rtk_array squadGladCards = RTKStruct_GetArray( model, "cards" )

	SquadSummaryData squadSummaryData = GetSquadSummaryData()

	entity localPlayer = GetLocalClientPlayer()
	EHI localPlayerEHI = -1
	if ( IsValid( localPlayer ) )
		localPlayerEHI = ToEHI( localPlayer )

	
	if ( squadSummaryData.playerData.len() > 1 )
	{
		SquadSummaryPlayerData playerToRepos

		foreach ( index, playerData in squadSummaryData.playerData)
		{
			if ( playerData.eHandle == localPlayerEHI)
			{
				playerToRepos = squadSummaryData.playerData.remove(index)
				break
			}
		}

		squadSummaryData.playerData.insert( 1, playerToRepos )
	}

	if ( RTKArray_GetCount( squadGladCards ) > 0 )
		RTKArray_Clear( squadGladCards )

	array< RTKSquadSummaryCardModel > cards

	foreach ( playerData in squadSummaryData.playerData)
	{
		bool isLocalPlayer = playerData.eHandle == localPlayerEHI
		entity dataPlayer = FromEHI( playerData.eHandle )

		
		RTKSquadSummaryCardModel cardModel

		EHIScriptStruct teammateEHISS
		if ( EEHHasValidScriptStruct( playerData.eHandle ) )
			teammateEHISS = GetEHIScriptStruct( playerData.eHandle )

		
		cardModel.isConnected = ( ( IsValid( dataPlayer ) )? dataPlayer.IsConnectionActive(): false )
		cardModel.playerEAID = teammateEHISS.playerEAID
		cardModel.uid = teammateEHISS.uid
		cardModel.hardware = teammateEHISS.hardware

		bool isPlayerBot = false
		bool canReport = false

		if ( IsValid( dataPlayer ) )
		{
#if AUTO_PLAYER
			isPlayerBot         = AutoPlayer_IsAutoPlayer( dataPlayer ) || dataPlayer.IsBot()
#else
			isPlayerBot         = dataPlayer.IsBot()
#endif

			cardModel.isMuted       = dataPlayer.IsVoiceAndTextMuted()
			cardModel.unspoofedUID  = dataPlayer.GetUnspoofedPlatformUserIdStr()
			canReport 				= CanReportPlayer( dataPlayer )
		}
		else
		{
			int reportStyle = GetReportStyle()

			switch ( reportStyle )
			{
				case 1: 
				canReport = CrossplayUserOptIn() ? true : PlatformIDToIconString(teammateEHISS.platformID) == GetLocalClientPlayer().GetHardwareName()
				break

				case 2: 
				canReport = true
				break

				case 0: 
				default:
					canReport = false
			}
		}

		cardModel.canViewProfile 	   = !isPlayerBot && !isLocalPlayer && Teams_CanViewProfile( teammateEHISS.uid, teammateEHISS.hardware )
		cardModel.canInvite            = !isPlayerBot && !isLocalPlayer && CanInviteSquadMate( cardModel.uid ) && CanInviteToparty() == 0
		cardModel.canMute              = !isPlayerBot && !isLocalPlayer && cardModel.isConnected
		cardModel.canReport            = !isPlayerBot && !isLocalPlayer && canReport
		cardModel.canSendFriendRequest = !isPlayerBot && !isLocalPlayer && CanSendFriendRequest( GetLocalClientPlayer() ) && !EADP_IsFriendByEAID( cardModel.playerEAID )

		if ( GetCurrentPlaylistVarBool( "showPlayerObfuscatedID", true ) && teammateEHISS.obfuscatedID != "" )
		{
			cardModel.obfuscatedID = teammateEHISS.obfuscatedID
		}

		if ( cardModel.canSendFriendRequest )
		{
			
			CommunityFriends friends = GetFriendInfo()
			foreach ( id in friends.ids )
			{
				if ( cardModel.uid == id )
				{
					cardModel.canSendFriendRequest = false
					break
				}
			}
		}

		string actionText = ""
		if( cardModel.canViewProfile )
			actionText += ( actionText == "" )? Localize( "#Y_BUTTON_VIEW_PROFILE" ): "\n" + Localize( "#Y_BUTTON_VIEW_PROFILE" )

		if( cardModel.canInvite )
			actionText += ( actionText == "" )? Localize( "#CLICK_INVITE_PARTY" ): "\n" + Localize( "#CLICK_INVITE_PARTY" )

		if( cardModel.canSendFriendRequest )
			actionText += ( actionText == "" )? Localize( "#RCLICK_INVITE_FRIEND" ): "\n" + Localize( "#RCLICK_INVITE_FRIEND" )

		cardModel.actionText = Localize( actionText )

		cardModel.platformIDString = PlatformIDToIconString( teammateEHISS.platformID )

		cardModel.playerName = GetDisplayablePlayerNameFromEHI( playerData.eHandle )
		cardModel.isLocalPlayer = isLocalPlayer

		
		array<RTKSquadSummaryCardStatModel> stats

		
		RTKSquadSummaryCardStatModel kak
		kak.title = "#DEATH_SCREEN_KILL_ASSISTS_KNOCKDOWNS"
		kak.value = Localize("#DEATH_SCREEN_KILL_ASSISTS_KNOCKDOWNS_VALUES", string( playerData.kills ), string( playerData.assists ), string( playerData.knockdowns ))

		stats.append(kak)

		
		RTKSquadSummaryCardStatModel damage
		damage.title = "#DEATH_SCREEN_SUMMARY_DAMAGE_DEALT"
		damage.value = string( playerData.damageDealt )

		stats.append(damage)

		
		RTKSquadSummaryCardStatModel survTime
		survTime.title = "#DEATH_SCREEN_SUMMARY_SURVIVAL_TIME"
		string minutes = string( playerData.survivalTime / 60 )
		string seconds = string ( playerData.survivalTime % 60 )
		if ( seconds.len() == 1 )
			seconds = "0" + seconds
		survTime.value =  Localize( "#DEATH_SCREEN_SUMMARY_SURVIVAL_TIME_FORMAT", minutes, seconds )

		stats.append(survTime)

		
		RTKSquadSummaryCardStatModel revives
		revives.title = "#DEATH_SCREEN_SUMMARY_REVIVES"
		revives.value = string( playerData.revivesGiven )

		stats.append(revives)

		
		RTKSquadSummaryCardStatModel respawnGiven
		respawnGiven.title = "#DEATH_SCREEN_SUMMARY_RESPAWNS"
		respawnGiven.value = string( playerData.respawnsGiven )

		stats.append(respawnGiven)

		cardModel.stats = stats

		cards.append( cardModel )
	}

	RTKArray_SetValue( squadGladCards, cards )
}

