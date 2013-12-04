<?php
    /* vim: set expandtab tabstop=4 shiftwidth=4 softtabstop=4: */

    /**
     * This file makes MediaWiki use a SMF user database to
     * authenticate with. This forces users to have a SMF account
     * in order to log into the wiki. This should also force the user to
     * be in a group called wiki.
     *
     * This program is free software; you can redistribute it and/or modify
     * it under the terms of the GNU General Public License as published by
     * the Free Software Foundation; either version 2 of the License, or
     * (at your option) any later version.
     *
     * This program is distributed in the hope that it will be useful,
     * but WITHOUT ANY WARRANTY; without even the implied warranty of
     * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
     * GNU General Public License for more details.
     *
     * You should have received a copy of the GNU General Public License along
     * with this program; if not, write to the Free Software Foundation, Inc.,
     * 59 Temple Place - Suite 330, Boston, MA 02111-1307, USA.
     * http://www.gnu.org/copyleft/gpl.html
     *
     * @package MediaWiki
     * @subpackage Auth_SMF
     * @author Nicholas Dunnaway
     * @copyright 2004-2006 php|uber.leet
     * @license http://www.gnu.org/copyleft/gpl.html
     * @CVS: $Id: Auth_SMF.php,v 1.3 2007/04/05 22:17:56 nkd Exp $
     * @link http://uber.leetphp.com
     * @version $Revision: 1.3 $
     *
     * 
     * @Modified By Hopesoft
     * @link http://www.51ajax.com
     * @2007-11-11
     *
     *
     * @Modified By outcrop
     * @email outcrop@163.com
     * @2008-04-07
     */

    error_reporting(0); // Debug
	include './extensions/Auth_UC/config.inc.php';
	include './extensions/Auth_UC/client/client.php';

    // First check if class has already been defined.
    if (!class_exists('AuthPlugin'))
    {
        /**
         * Auth Plugin
         *
         */
        require_once './includes/AuthPlugin.php';
    }

    /**
     * Handles the Authentication with the Discuz database.
     *
     * @package MediaWiki
     * @subpackage Auth_Discuz
     */
    class Auth_Discuz extends AuthPlugin
    {
    	var $username;
		function username($username)
		{
			$username = trim($_POST['wpName']) ? trim($_POST['wpName']) : $username ;
			$username = htmlentities($username, ENT_QUOTES, 'UTF-8');
			$username = str_replace('&#039;', '\\\'', $username);
			$this->username = $username;
			return $username;
		}
        /**
         * Add a user to the external authentication database.
         * Return true if successful.
         *
         * NOTE: We are not allowed to add users to Discuz from the
         * wiki so this always returns false.
         *
         * @param User $user
         * @param string $password
         * @return bool
         * @access public
         */
        function addUser( $user, $password )
        {
            return false;
        }

		/**
		 * Can users change their passwords?
		 *
		 * @return bool
		 */
		function allowPasswordChange()
		{
			return true;
		}

        /**
         * Check if a username+password pair is a valid login.
         * The name will be normalized to MediaWiki's requirements, so
         * you might need to munge it (for instance, for lowercase initial
         * letters).
         *
         * @param string $username
         * @param string $password
         * @return bool
         * @access public
         * @todo Check if the password is being changed when it contains a slash or an escape char.
         */
        function authenticate($username, $password)
        {
            list($uid, $username1, $password, $email) = uc_user_login($this->username($username), $password);
            
            if ($uid > 0) {
            	return true;
            }
            if ($uid== -1){
            	return false;
            }
            if ($uid== -2){
            	return false;
            }
        }

        /**
         * Return true if the wiki should create a new local account automatically
         * when asked to login a user who doesn't exist locally but does in the
         * external auth database.
         *
         * If you don't automatically create accounts, you must still create
         * accounts in some way. It's not possible to authenticate without
         * a local account.
         *
         * This is just a question, and shouldn't perform any actions.
         *
         * NOTE: I have set this to true to allow the wiki to create accounts.
         *       Without an accout in the wiki database a user will never be
         *       able to login and use the wiki. I think the password does not
         *       matter as long as authenticate() returns true.
         *
         * @return bool
         * @access public
         */
        function autoCreate()
        {
            return true;
        }

        /**
         * Check to see if external accounts can be created.
         * Return true if external accounts can be created.
         *
         * NOTE: We are not allowed to add users to Discuz from the
         * wiki so this always returns false.
         *
         * @return bool
         * @access public
         */
        function canCreateAccounts()
        {
            return false;
        }
        
        /**
         * Connect to the database. All of these settings are from the
         * LocalSettings.php file. This assumes that the Discuz uses the same
         * database/server as the wiki.
         *
         * {@source }
         * @return resource
         */
      
         /**
         * If you want to munge the case of an account name before the final
         * check, now is your chance.
         */
        function getCanonicalName( $username )
        {
            return $username;
        }

        /**
         * When creating a user account, optionally fill in preferences and such.
         * For instance, you might pull the email address or real name from the
         * external user database.
         *
         * The User object is passed by reference so it can be modified; don't
         * forget the & on your function declaration.
         *
         * NOTE: This gets the email address from Discuz for the wiki account.
         *
         * @param User $user
         * @access public
         */
        function initUser(&$user)
        {
	        if($data = uc_get_user($this->username($user->mName))) {
				list($uid, $username1, $email) = $data;
				$user->mEmail	= $email;
				$user->mid		= $uid;
				$user->mName	= $username1;
			} else {
				echo 'No such user for initial: '.$user;
			}
        }

        /**
         * Modify options in the login template.
         *
         * NOTE: Turned off some Template stuff here. Anyone who knows where
         * to find all the template options please let me know. I was only able
         * to find a few.
         *
         * @param UserLoginTemplate $template
         * @access public
         */
        function modifyUITemplate( &$template )
        {
            $template->set('usedomain',   false); // We do not want a domain name.
            $template->set('create',      false); // Remove option to create new accounts from the wiki.
            $template->set('useemail',    false); // Disable the mail new password box.
        }

        /**
         * This prints an error when a MySQL error is found.
         *
         * @param string $message
         * @access public
         */
        function mySQLError( $message )
        {
            echo $message . '<br />';
            echo 'MySQL Error Number: ' . mysql_errno() . '<br />';
            echo 'MySQL Error Message: ' . mysql_error() . '<br /><br />';
            exit;
        }

        /**
         * Set the domain this plugin is supposed to use when authenticating.
         *
         * NOTE: We do not use this.
         *
         * @param string $domain
         * @access public
         */
        function setDomain( $domain )
        {
            $this->domain = $domain;
        }

    	/**
    	 * Set the given password in the authentication database.
    	 * Return true if successful.
    	 *
    	 * NOTE: We only allow the user to change their password via phpBB.
    	 *
    	 * @param string $password
    	 * @return bool
    	 * @access public
    	 */
    	function setPassword( $password )
    	{
    		return true;
    	}

        /**
         * Return true to prevent logins that don't authenticate here from being
         * checked against the local database's password fields.
         *
         * This is just a question, and shouldn't perform any actions.
         *
         * Note: This forces a user to pass Authentication with the above
         *       function authenticate(). So if a user changes their Discuz
         *       password, their old one will not work to log into the wiki.
         *       Wiki does not have a way to update it's password when Discuz
         *       does. This however does not matter.
         *
         * @return bool
         * @access public
         */
        function strict()
        {
            return true;
        }

		/**
		 * Update user information in the external authentication database.
		 * Return true if successful.
		 *
		 * @param $user User object.
		 * @return bool
		 * @public
		 */
		function updateExternalDB( $user )
		{
			return true;
		}

        /**
         * When a user logs in, optionally fill in preferences and such.
         * For instance, you might pull the email address or real name from the
         * external user database.
         *
         * The User object is passed by reference so it can be modified; don't
         * forget the & on your function declaration.
         *
         * NOTE: Not useing right now.
         *
         * @param User $user
         * @access public
         */
        function updateUser( &$user )
        {
            return true;
        }

        /**
         * Check whether there exists a user account with the given name.
         * The name will be normalized to MediaWiki's requirements, so
         * you might need to munge it (for instance, for lowercase initial
         * letters).
         *
         * NOTE: MediaWiki checks its database for the username. If it has
         *       no record of the username it then asks. "Is this really a
         *       valid username?" If not then MediaWiki fails Authentication.
         *
         * @param string $username
         * @return bool
         * @access public
         * @todo write this function.
         */
        function userExists($username) //判断用户是否在Ucenter中存在
        {
			if($data = uc_get_user($this->username($username))) {
				list($uid, $username1, $email) = $data;	
				return true;
			} else {
				return false;
			}

        }

        /**
         * Check to see if the specific domain is a valid domain.
         *
         * @param string $domain
         * @return bool
         * @access public
         */
        function validDomain( $domain )
        {
            return true;
        }

    }

?>