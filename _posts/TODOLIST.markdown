---
title : JBoss服务器在servlet中处理异常信息
---

遗留问题，下级服务器查询条件问题。

更改策略应用后，没有变化，

URL 区分大小写。

有些目录不可以进的，但是目录里有的文件是可读的，可以用程序直接选择路径文件。

Jboss不熟war包出错

对界面的要求要高

遇到问题，及时处理。

遗留代码、版本处理。

```
18:38:03,937 ERROR [ProfileDeployAction] Failed to add deployment: wbconsole.war: org.jboss.deployers.spi.DeploymentException: Failed to mount archive: "/E:/UEM/WBServer/server/default/deploy/wbconsole.war"
	at org.jboss.deployers.spi.DeploymentException.rethrowAsDeploymentException(DeploymentException.java:49) [:2.2.0.GA]
	at org.jboss.deployers.vfs.plugins.structure.AbstractVFSArchiveStructureDeployer.mountArchive(AbstractVFSArchiveStructureDeployer.java:132) [:2.2.0.GA]
	at org.jboss.deployers.vfs.plugins.structure.AbstractVFSArchiveStructureDeployer.determineStructure(AbstractVFSArchiveStructureDeployer.java:55) [:2.2.0.GA]
	at org.jboss.deployers.vfs.plugins.structure.StructureDeployerWrapper.determineStructure(StructureDeployerWrapper.java:73) [:2.2.0.GA]
	at org.jboss.deployers.vfs.plugins.structure.VFSStructuralDeployersImpl.doDetermineStructure(VFSStructuralDeployersImpl.java:197) [:2.2.0.GA]
	at org.jboss.deployers.vfs.plugins.structure.VFSStructuralDeployersImpl.determineStructure(VFSStructuralDeployersImpl.java:222) [:2.2.0.GA]
	at org.jboss.deployers.structure.spi.helpers.AbstractStructuralDeployers.determineStructure(AbstractStructuralDeployers.java:77) [:2.2.0.GA]
	at org.jboss.deployers.plugins.main.MainDeployerImpl.determineStructure(MainDeployerImpl.java:1106) [:2.2.0.GA]
	at org.jboss.deployers.plugins.main.MainDeployerImpl.determineDeploymentContext(MainDeployerImpl.java:417) [:2.2.0.GA]
	at org.jboss.deployers.plugins.main.MainDeployerImpl.addDeployment(MainDeployerImpl.java:367) [:2.2.0.GA]
	at org.jboss.deployers.plugins.main.MainDeployerImpl.addDeployment(MainDeployerImpl.java:277) [:2.2.0.GA]
	at org.jboss.system.server.profileservice.deployers.MainDeployerPlugin.addDeployment(MainDeployerPlugin.java:77) [:6.0.0.Final]
	at org.jboss.profileservice.dependency.ProfileControllerContext$DelegateDeployer.addDeployment(ProfileControllerContext.java:133) [:0.2.2]
	at org.jboss.profileservice.dependency.ProfileDeployAction.deploy(ProfileDeployAction.java:132) [:0.2.2]
	at org.jboss.profileservice.dependency.ProfileDeployAction.installActionInternal(ProfileDeployAction.java:94) [:0.2.2]
	at org.jboss.kernel.plugins.dependency.InstallsAwareAction.installAction(InstallsAwareAction.java:54) [jboss-kernel.jar:2.2.0.GA]
	at org.jboss.kernel.plugins.dependency.InstallsAwareAction.installAction(InstallsAwareAction.java:42) [jboss-kernel.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.action.SimpleControllerContextAction.simpleInstallAction(SimpleControllerContextAction.java:62) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.action.AccessControllerContextAction.install(AccessControllerContextAction.java:71) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractControllerContextActions.install(AbstractControllerContextActions.java:51) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractControllerContext.install(AbstractControllerContext.java:379) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.install(AbstractController.java:2044) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.incrementState(AbstractController.java:1083) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.executeOrIncrementStateDirectly(AbstractController.java:1322) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.resolveContexts(AbstractController.java:1246) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.resolveContexts(AbstractController.java:1139) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.change(AbstractController.java:939) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.dependency.plugins.AbstractController.change(AbstractController.java:654) [jboss-dependency.jar:2.2.0.GA]
	at org.jboss.profileservice.dependency.ProfileActivationWrapper$BasicProfileActivation.start(ProfileActivationWrapper.java:190) [:0.2.2]
	at org.jboss.profileservice.dependency.ProfileActivationWrapper.start(ProfileActivationWrapper.java:87) [:0.2.2]
	at org.jboss.profileservice.dependency.ProfileActivationService.activateProfile(ProfileActivationService.java:215) [:0.2.2]
	at org.jboss.profileservice.dependency.ProfileActivationService.activate(ProfileActivationService.java:159) [:0.2.2]
	at org.jboss.profileservice.bootstrap.AbstractProfileServiceBootstrap.activate(AbstractProfileServiceBootstrap.java:112) [:0.2.2]
	at org.jboss.profileservice.resolver.BasicResolverFactory$ProfileResolverFacade.deploy(BasicResolverFactory.java:87) [:0.2.2]
	at org.jboss.profileservice.bootstrap.AbstractProfileServiceBootstrap.start(AbstractProfileServiceBootstrap.java:91) [:0.2.2]
	at org.jboss.system.server.profileservice.bootstrap.BasicProfileServiceBootstrap.start(BasicProfileServiceBootstrap.java:132) [:6.0.0.Final]
	at org.jboss.system.server.profileservice.bootstrap.BasicProfileServiceBootstrap.start(BasicProfileServiceBootstrap.java:56) [:6.0.0.Final]
	at org.jboss.bootstrap.impl.base.server.AbstractServer.startBootstraps(AbstractServer.java:827) [jboss-bootstrap-impl-base.jar:2.1.0-alpha-5]
	at org.jboss.bootstrap.impl.base.server.AbstractServer$StartServerTask.run(AbstractServer.java:417) [jboss-bootstrap-impl-base.jar:2.1.0-alpha-5]
	at java.lang.Thread.run(Thread.java:662) [:1.6.0_37]
Caused by: java.util.zip.ZipException: invalid stored block lengths
	at java.util.zip.InflaterInputStream.read(InflaterInputStream.java:147) [:1.6.0_37]
	at java.io.FilterInputStream.read(FilterInputStream.java:90) [:1.6.0_37]
	at org.jboss.vfs.VFSUtils.copyStream(VFSUtils.java:426) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.VFSUtils.copyStream(VFSUtils.java:406) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.VFSUtils.unzip(VFSUtils.java:826) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.VFS.mountZipExpanded(VFS.java:556) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.VFS.mountZipExpanded(VFS.java:587) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.util.automount.Automounter$RegistryEntry.mount(Automounter.java:230) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.util.automount.Automounter$RegistryEntry.access$000(Automounter.java:208) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.util.automount.Automounter.mount(Automounter.java:117) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.vfs.util.automount.Automounter.mount(Automounter.java:77) [jboss-vfs.jar:3.0.0.GA]
	at org.jboss.web.deployers.WARStructure.performMount(WARStructure.java:264) [:6.0.0.Final]
	at org.jboss.deployers.vfs.plugins.structure.AbstractVFSArchiveStructureDeployer.mountArchive(AbstractVFSArchiveStructureDeployer.java:128) [:2.2.0.GA]
	... 38 more
```

是war出现损坏，用解压软件解压war包，发现出现错误。