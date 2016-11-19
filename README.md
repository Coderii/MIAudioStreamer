# MTAudioStreamer

> ʹ��demoǰ�ر�ע�⡣��Ϊ�ҵ������ļ������ǹ�����ţ�������ϵģ���ʱ����url��ʧЧ������demo���������ļ���ʾ����Ч����

> ����������ϸ��ʾ���뽫���°��е�initial viewController����ΪviewController�������Ҫ��ʾ������ֵ�Ч�����뽫initial viewController����ΪMTNavigationController��

> һ��audioFileֻ�ܳ�ʼ��һ��MusicFeature�������ܱ�֤�жϾ�ȷ�Ƿ��Ѿ����ع��Ͳ��Ź�

### ���
	
- MTAudioStreamer�ǻ���DOUAudioStreamer�����޸ģ���ԭ����Ͻ���һЩ�Ż��Ĳ���������Ĺ������¡�

#### ��Ҫ����
- ������Ź��ܣ�ͬһ�������ٵ�����أ�ֱ�ӴӲ��Ż���Ľ��ȿ�ʼ��������

- ������ع��ܣ�ͬһ�������ٵ�����ţ����ŵĻ������ֱ�Ӵ��Ѿ����صĽ��ȿ�ʼ����

- ֧�ֶϵ�����������������һ��رպ�̨��ֻҪĿ¼�����ļ����ڣ�����Ҫ�����������������

- ֧�ֶ��й���������󲢷�������������һ�������ص��������� 

- ���ܻ������������ɺ�ֱ�ӱ���Ϊ�����ļ�

### ʹ��˵��
#### ����Ҫ��
* iOS�汾Ҫ��: >= 7.0

#### ���벽��
* ��ԭ���Ŀ�����޸ģ���Ҫֱ�Ӱ����޸ĺ������Դ�뵼�뵽��Ŀ�У���Ҫ�Լ���ƶ��й������ֻ��Ҫֱ�ӵ���MTMusicFeature.h�ļ����ɣ�Ȼ�����Լ��Ķ�����ѡ����Ҫ�ķ�����

	* ��Ҫ�Լ��̳�DOUAudioFileЭ���ʼ��model

	    	NSURL *url = [NSURL URLWithString:@" "];
			Track *track = [[Track alloc] init];
    		track.audioFileURL = url;
    		
    * ��ʼ��MTMusicFeature

	    	self.musicFeature = [[MTMusicFeature alloc] initMusicFeatureWithAudioFile:track];
	    	
	* ��ʼ���ֲ��Ź���

			 [self.musicFeature startWithType:MTMusicFeatureTypePlay];
			 
	* ���Ż�����ȷ���

			[self.musicFeature setPlayCacheProgress:^(double progress) {
				// ��������Բ�����������ʾ
	        }];
	 	
	* ��ʼ�������ع���

			 [self.musicFeature startWithType:MTMusicFeatureTypeDownload];
			 
	* ���ؽ��ȷ���

	        [self.musicFeature setDownloadProgress:^(double progress) {
	        	// ��������Բ�����������ʾ
    	    }];
    	    
   	* �жϵ�ǰ�����URL�Ƿ��ڱ��ش����ļ������ػ��߻�����ɹ���

		    if (self.musicFeature.localFileExist) {
		    	// ������ش���ֱ��������Ҫ�Ľ�������ʾ100%
		    	self.downloadProgressView.progress = 1.0f;
        	}
        	
* ���ʹ���ҷ�װ�Ķ��С���ֱ�ӵ���MTOperationQueueManager.h�ļ������ʹ�øö��й���
> ��Ҫע����ǣ�����model�ļ���ʱ����Ҫ�̳�MTDownLoadFileProtocolЭ�顣

	* ��ʼ������

			 @interface Track : NSObject <DOUAudioFile, MTDownLoadFileProtocol>
			 
	* ��Ҫ����һ��audiofile��Ӧ��һ��musicFeatur��������������

			[_musicFeatureModelDict setObject:self.musicFeature forKey:[track audioFileURL].absoluteString];
			
	* �����ط�����й�������model��model��Ӧ��һ��musicFeature

			[[MTMusicOperationManager sharedManager] startDownloadWithMusicModel:track
                                                            musicFeature:[_musicFeatureModelDict objectForKey:[track audioFileURL].absoluteString]];
			
	* ������ǰ���������صĽ���

			[track.operation setProgressFeadBackBlock:^(double progress) {
				// ������ʹ��
			}];
			
	* ���������Ӳ��Ž����У�ʼ�ձ��ֲ��ŵĲ������ȼ����

		    [[MTMusicOperationManager sharedManager] startPlayOperation:invocationOperation newDownloadProgress:^(double progress, id<MTDownLoadFileProtocol> model) { 
		    		//������ź󷵻��µĻ�����Ⱥ�֮ǰ���ع���model���������ڲ������½�������
		    }];
		
### ��Ҫ�ļ����ļ�����

#### MTAudioHTTPRequest.h
- ��Ҫ����������⣬ʵ���˶ϵ������Ĺ��ܡ��滻����ԭ��DOUAudioStreamer�еĻ���CFNetworkд������⣬ʵ����ԭ��DOUSimpleHTTPRequest�еķ��������ڴ˻�������ӵĶϵ��������ܡ�



#### MTConstant.h

* ��������Ҫ�Ĺ�ͬ������������

#### MTAudioDownloadProvider.h
- �Ӳ������г�ȡ�����������صĽӿ��ļ������棩

	- ����AudioFile��ʼ��MTAudioDownloadProvider
			
			self.downloadProvider = [[MTAudioDownloadProvider alloc] initAudioDownloadProviderWithAudioFile:_audioFile];

	- ��ʼ����

			[self.downloadProvider start];
			
	- ��ͣ����

			[self.streamer pause];
			
	- ֹͣ����

			[self.streamer cancel];
			
	- �����ӿ�
	
			MTAudioDownloadProviderProgress //���ؽ��Ȼص�Block
			MTAudioDownloadProviderCompleted //������ɵĻص�Block
			
#### MTMusicFeature.h
- ��Ҫ�ķ�װ�ӿڣ�������ӿ��ڴ����ź����ز������߼��������Ҫ��ҵ���߼����ܡ�����MTMusicFeatureTypeö��ֵѡ��ǰ�ǲ��Ż������ع��ܡ�

#### NSString+MTSH256Path.h
- NSString�ķ��࣬��Ҫ���ڻ�ȡ�ļ���·�����Լ���SH256����ǩ���õ��ļ�����һЩ������

### ��������